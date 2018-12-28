# Level 1 Hardening - Enable TLS on the Kubernetes API

A huge thanks to [Kelsey Hightower's Kubernetes the Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way/blob/master/docs/04-certificate-authority.md) for paving the way on this part.  Be sure to have CloudFlare's PKI toolkit [cfssl](https://github.com/cloudflare/cfssl) [installed on your system](https://github.com/kelseyhightower/kubernetes-the-hard-way/blob/master/docs/02-client-tools.md).  

## Generate the Certificate Authority

Create the CA configuration file:

```
cat > ca-config.json <<EOF
{
  "signing": {
    "default": {
      "expiry": "8760h"
    },
    "profiles": {
      "kubernetes": {
        "usages": ["signing", "key encipherment", "server auth", "client auth"],
        "expiry": "8760h"
      }
    }
  }
}
EOF
```

Create the configuration file needed for the CA certificate signing request:
```
cat > ca-csr.json <<EOF
{
  "CN": "Kubernetes",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "Kubernetes",
      "OU": "CA",
      "ST": "Oregon"
    }
  ]
}
EOF
```

Generate the CA certificate and CA private key:
```
cfssl gencert -initca ca-csr.json | cfssljson -bare ca
```

Which creates:
```
ca-key.pem
ca.pem
```

## Generate the Admin Key Pair

Create the configuration file needed for the `admin` client certificate signing request:
```
cat > admin-csr.json <<EOF
{
  "CN": "admin",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "system:masters",
      "OU": "Kubernetes",
      "ST": "Oregon"
    }
  ]
}
EOF
```

Generate the `admin` client certificate and private key:
```
cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  admin-csr.json | cfssljson -bare admin
```

Which creates:
```
admin-key.pem
admin.pem
```

## Create the `admin` user kubeconfig for `kubectl` access

```
$ export AWS_DEFAULT_REGION="us-east-1"
$ export CONTROLLER_IP=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=controller' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

To contain this configuration to the local repository's folder, be sure to set the `KUBECONFIG` environment variable:
```
$ export KUBECONFIG=./kubeconfig
```
Generate a kubeconfig file suitable for authenticating as the `admin` user.  
```
$ kubectl config set-cluster hardening-kubernetes \
  --certificate-authority=ca.pem \
  --embed-certs=true \
  --server=https://${CONTROLLER_IP}:6443
```

```
$ kubectl config set-credentials admin \
  --client-certificate=admin.pem \
  --client-key=admin-key.pem
```

```
$ kubectl config set-context hardening-kubernetes \
  --cluster=hardening-kubernetes \
  --user=admin
```

```
$ kubectl config use-context hardening-kubernetes
```

## Generate the Kubernetes API Key Pair

The `CONTROLLER_IP` address will be included in the list of Subject Alternative Names for the Kubernetes API Server certificate. This will ensure the certificate can be validated by remote clients.

Create the Kubernetes API Server certificate signing request:

```
cat > kubernetes-csr.json <<EOF
{
  "CN": "kubernetes",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "Kubernetes",
      "OU": "Kubernetes",
      "ST": "Oregon"
    }
  ]
}
EOF
```

Generate the Kubernetes API Server certificate and private key using the various IPs and Hostnames used to reach the controller instance:
```
$ cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -hostname=mykubecluster,10.3.0.1,10.1.0.10,${CONTROLLER_IP},127.0.0.1,kubernetes.default \
  -profile=kubernetes \
  kubernetes-csr.json | cfssljson -bare kubernetes
```

Which creates:
```
kubernetes-key.pem
kubernetes.pem
```

## Configure the Kubernetes API Server for TLS

SCP the files needed up to the `CONTROLLER` node:

```
$ export AWS_DEFAULT_REGION="us-east-1"
$ export KEY_NAME="hkfs"
$ scp -i ${KEY_NAME}.pem ca*pem ubuntu@${CONTROLLER_IP}:
$ scp -i ${KEY_NAME}.pem kubernetes*pem ubuntu@${CONTROLLER_IP}:
```

SSH into the `CONTROLLER` node:

```
$ ssh -i ${KEY_NAME}.pem ubuntu@${CONTROLLER_IP}
controller$ sudo mkdir -p /var/lib/kubernetes/
controller$ sudo mv ca.pem ca-key.pem kubernetes-key.pem kubernetes.pem /var/lib/kubernetes/
```
The following modifications/additions are needed to the `kube-apiserver.service` `systemd unit` file:
- `bind-address`: Tell the API Server what address to listen on for the TLS-enabled API service.
- `client-ca-file`: Provide the API Server with a CA cert to use during validation of TLS client certificates and to extract the `CommonName`.
- `secure-port`: Tell the API Server what port to listen on for the TLS-enabled API service.
- `service-account-key-file`: The CA private key used to verify ServiceAccount tokens.
- `tls-ca-file`:  The CA cert used for secure access from Admission Controllers.
- `tls-cert-file`: The `kubernetes` public key used for HTTPS/TLS.
- `tls-cert-key`: The `kubernetes` private key used for HTTPS/TLS.

Modify the Kubernetes API Server `systemd unit` to add these changes in one shot:
```
controller$ cat > kube-apiserver.service <<EOF
[Unit]
Description=Kubernetes API Server
Documentation=https://github.com/kubernetes/kubernetes
[Service]
ExecStart=/usr/local/bin/kube-apiserver \\
  --admission-control=AlwaysAdmit \\
  --advertise-address=10.1.0.10 \\
  --allow-privileged=true \\
  --apiserver-count=1 \\
  --authorization-mode=AlwaysAllow \\
  --bind-address=0.0.0.0 \\
  --client-ca-file=/var/lib/kubernetes/ca.pem \\
  --etcd-servers=http://10.1.0.5:2379 \\
  --insecure-bind-address=0.0.0.0 \\
  --runtime-config="api/all" \\
  --secure-port=6443 \\
  --service-account-key-file=/var/lib/kubernetes/ca-key.pem \\
  --service-cluster-ip-range=10.3.0.0/16 \\
  --service-node-port-range=30000-32767 \\
  --tls-ca-file=/var/lib/kubernetes/ca.pem \\
  --tls-cert-file=/var/lib/kubernetes/kubernetes.pem \\
  --tls-private-key-file=/var/lib/kubernetes/kubernetes-key.pem \\
  --v=2
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
EOF

controller$ sudo mv kube-apiserver.service /etc/systemd/system/
```

Restart the Kubernetes API Server service:

```
controller$ sudo systemctl daemon-reload
controller$ sudo systemctl restart kube-apiserver
```

Validate that it's running:

```
controller$ netstat -nat | grep 6443
tcp        0      0 127.0.0.1:57040         127.0.0.1:6443          ESTABLISHED
tcp6       0      0 :::6443                 :::*                    LISTEN     
tcp6       0      0 127.0.0.1:6443          127.0.0.1:57040         ESTABLISHED

controller$ exit
```

## Validate TLS-enabled `kubectl` API Server Access

Check the health of the remote Kubernetes cluster:
```
$ kubectl config use-context hardening-kubernetes
Switched to context "hardening-kubernetes".
$ kubectl get nodes
NAME           STATUS    ROLES     AGE       VERSION
ip-10-1-0-10   Ready     <none>    16d       v1.9.2
ip-10-1-0-11   Ready     <none>    16d       v1.9.2
ip-10-1-0-12   Ready     <none>    16d       v1.9.2
```

Fantastic!  From outside the cluster, we only allow SSH (`tcp/22`) and client-certificates are now need to access the Kubernetes API Server over TLS (`tcp/6443`).  All sorts of attacks are now thwarted—and we're done, right?  Well, depending on the workload and the access to the cluster, it *may* be sufficient, but we'll test those assumptions.

[Back](/README.md#level-1-hardening) | [Next](deploy-vulnapp.md)
