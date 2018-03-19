# Launch and Configure the ```controller``` Instance

## Instance Creation
From the same shell on the installation system, create the ```controller``` instance
```
$ aws ec2 run-instances \
  --region ${AWS_DEFAULT_REGION} \
  --image-id ${IMAGE_ID} \
  --count 1 \
  --instance-type t2.small \
  --key-name ${KEY_NAME} \
  --subnet-id ${SUBNET_ID} \
  --associate-public-ip-address \
  --query 'Instances[0].InstanceId' \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=controller}]' \
  --private-ip-address 10.1.0.10 \
  --block-device-mapping 'DeviceName=/dev/sda1,Ebs={VolumeSize=32}' \
  --output text
```

Disable Source/Destination Checking for ```kube-proxy```
```
$ aws ec2 modify-instance-attribute \
  --region ${AWS_DEFAULT_REGION} \
  --no-source-dest-check \
  --instance-id "$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=controller' \
  --query 'Reservations[].Instances[].InstanceId' \
  --output text)"
```

## Installation and Configuration

SSH Into the ```controller``` Instance
```
$ ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=controller' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

Install Docker and Other Necessary Binaries
```
$ sudo apt-get update
$ sudo apt-get install docker.io socat conntrack --yes
```

Configure and Start Docker
```
$ echo "DOCKER_OPTS=--ip-masq=false --iptables=false \
--log-driver=json-file --log-level=warn --log-opt=max-file=5 \
--log-opt=max-size=10m \
--storage-driver=overlay" | sudo tee -a /etc/default/docker
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker.service
```

Verify Docker is Running
```
$ sudo docker ps
```

Enable Forwarding for ```kube-proxy``` Functions
```
$ sudo iptables -P FORWARD ACCEPT
```

Download the Kubernetes Binaries
```
$ export K8S_RELEASE="1.9.2"
$ wget -q --show-progress --https-only --timestamping \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-apiserver" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-controller-manager" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-scheduler" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kubectl" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-proxy" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kubelet"
```

Make the Kubernetes Binaries Executable and Place them in the PATH
```
$ chmod +x kube-apiserver kube-controller-manager kube-scheduler kubectl kubelet kube-proxy
$ sudo mv kube-apiserver kube-controller-manager kube-scheduler \
  kubectl kubelet kube-proxy /usr/local/bin/
```

Create Supporting Directories
```
$ sudo mkdir -p \
  /etc/cni/net.d \
  /opt/cni/bin \
  /var/lib/kubelet \
  /var/lib/kube-proxy \
  /var/lib/kubernetes \
  /var/run/kubernetes
```

Download the CNI Plugins for ```kubenet```
```
$ wget -q --show-progress --https-only --timestamping \
  "https://github.com/containernetworking/plugins/releases/download/v0.6.0/cni-plugins-amd64-v0.6.0.tgz"
```

Install the CNI Plugins
```
$ sudo tar -xvf cni-plugins-amd64-v0.6.0.tgz -C /opt/cni/bin/
```

Configure the ```kube-apiserver``` Systemd Unit
```
$ cat > kube-apiserver.service <<EOF
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
  --etcd-servers=http://10.1.0.5:2379 \\
  --insecure-bind-address=0.0.0.0 \\
  --runtime-config=api/all \\
  --service-cluster-ip-range=10.3.0.0/16 \\
  --service-node-port-range=30000-32767 \\
  --v=2
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
EOF
```

Configure the ```kube-controller-manager``` Systemd Unit
```
$ cat > kube-controller-manager.service <<EOF
[Unit]
Description=Kubernetes Controller Manager
Documentation=https://github.com/kubernetes/kubernetes
[Service]
ExecStart=/usr/local/bin/kube-controller-manager \\
  --address=0.0.0.0 \\
  --cluster-cidr=10.2.0.0/16 \\
  --cluster-name=kubernetes \\
  --master=http://10.1.0.10:8080 \\
  --v=2
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
EOF
```

Configure the ```kube-scheduler``` Systemd Unit
```
$ cat > kube-scheduler.service <<EOF
[Unit]
Description=Kubernetes Scheduler
Documentation=https://github.com/kubernetes/kubernetes
[Service]
ExecStart=/usr/local/bin/kube-scheduler \\
  --master=http://10.1.0.10:8080 \\
  --v=2
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
EOF
```

Configure the ```kubelet``` Systemd Unit
```
$ cat > kubelet.service <<EOF
[Unit]
Description=Kubernetes Kubelet
Documentation=https://github.com/kubernetes/kubernetes
[Service]
ExecStart=/usr/local/bin/kubelet \\
  --allow-privileged=true \\
  --cluster-dns=10.3.0.10 \\
  --cluster-domain=cluster.local \\
  --kubeconfig=/var/lib/kubelet/kubeconfig \\
  --network-plugin=kubenet \\
  --non-masquerade-cidr=10.2.0.0/16 \\
  --pod-cidr=10.2.0.0/24 \\
  --register-node=true \\
  --runtime-request-timeout=15m \\
  --v=2
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
EOF
```

Create the ```kubelet``` ```kubeconfig``` File
```
$ cat > kubeconfig <<EOF
apiVersion: v1
clusters:
- cluster:
    server: http://10.1.0.10:8080
  name: default
contexts:
- context:
    cluster: default
    user: system:node:master
  name: default
current-context: "default"
kind: Config
preferences: {}
users:
- name: system:node:master
  user:
    as-user-extra: {}
EOF
```

Configure the ```kube-proxy``` Systemd Unit
```
$ cat > kube-proxy.service <<EOF
[Unit]
Description=Kubernetes Kube Proxy
Documentation=https://github.com/kubernetes/kubernetes
[Service]
ExecStart=/usr/local/bin/kube-proxy \\
  --cluster-cidr=10.2.0.0/16 \\
  --master=http://10.1.0.10:8080 \\
  --v=2
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
EOF
```

Place the Systemd Units and Start All Services
```
$ sudo mv kubeconfig /var/lib/kubelet/kubeconfig
$ sudo mv kube-apiserver.service kube-scheduler.service kube-controller-manager.service \
  kubelet.service kube-proxy.service /etc/systemd/system/
$ sudo systemctl daemon-reload
$ sudo systemctl enable kube-apiserver kube-controller-manager kube-scheduler kubelet kube-proxy
$ sudo systemctl restart kube-apiserver kube-controller-manager kube-scheduler kubelet kube-proxy
```

Exit the ```controller``` instance SSH session to return to the installation system shell.

## Routing the Pod CIDR for ```kubenet```

Obtain the Route Table ID
```
$ ROUTETABLE_ID=$(aws ec2 describe-route-tables \
  --region ${AWS_DEFAULT_REGION} \
  --filter "Name=tag:Name,Values=${STACK_NAME}-rt" \
  --query 'RouteTables[*].RouteTableId' \
  --output text)
```

Obtain the ```controller``` ENI ID
```
$ CONTROLLERENI_ID=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=controller' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].NetworkInterfaceId' \
  --output text)
```

Add the ```controller``` Pod CIDR Route to the Route Table
```
$ aws ec2 create-route \
  --region ${AWS_DEFAULT_REGION} \
  --route-table-id ${ROUTETABLE_ID} \
  --network-interface-id ${CONTROLLERENI_ID} \
  --destination-cidr-block '10.2.0.0/24' \
  --output text
```

## Validation

SSH Into the ```controller``` Instance
```
$ ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=controller' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

Verify that the cluster control plane components are running and healthy.
```
$ kubectl get componentstatuses
```

Example of healthy output:
```
NAME                 STATUS    MESSAGE              ERROR
controller-manager   Healthy   ok
scheduler            Healthy   ok
etcd-0               Healthy   {"health": "true"}
```

Exit the ```controller``` instance SSH session to return to the installation system shell.

[Back](/README.md) | [Next](launch-configure-workers.md)
