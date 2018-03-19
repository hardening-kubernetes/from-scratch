# Launch and Configure the ```worker-1``` and ```worker-2``` Instances

## ```worker-1``` Instance Creation

From the same installation system shell, create the ```worker-1``` instance
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
  --output text \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=worker-1}]' \
  --private-ip-address 10.1.0.11 \
  --block-device-mapping 'DeviceName=/dev/sda1,Ebs={VolumeSize=32}'
```

Disable Source/Destination Checking for ```kube-proxy```
```
$ aws ec2 modify-instance-attribute \
  --region ${AWS_DEFAULT_REGION} \
  --no-source-dest-check \
  --instance-id "$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=worker-1' \
  --query 'Reservations[].Instances[].InstanceId' \
  --output text)"
```

When the instance is running, SSH in.
```
$ ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=worker-1' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

## Installation and Configuration

Install Docker and Other Necessary Binaries
```
$ sudo apt-get update
$ sudo apt-get install docker.io socat conntrack --yes
```

Configure and Start Docker
```
$ echo "DOCKER_OPTS=--ip-masq=false --iptables=false \
--log-driver=json-file --log-level=warn \
--log-opt=max-file=5 --log-opt=max-size=10m \
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

Download the Kubernetes Binaries
```
$ export K8S_RELEASE="1.9.2"
$ wget -q --show-progress --https-only --timestamping \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-proxy" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kubelet"
```

Make the Kubernetes Binaries Executable and Place them in the PATH
```
$ chmod +x kube-proxy kubelet
$ sudo mv kube-proxy kubelet /usr/local/bin/
```

Configure the ```kubelet``` Systemd Unit
```
$ cat > kubelet.service <<EOF
[Unit]
Description=Kubernetes Kubelet
Documentation=https://github.com/kubernetes/kubernetes
[Service]
ExecStart=/usr/local/bin/kubelet \
  --allow-privileged=true \
  --cluster-dns=10.3.0.10 \
  --cluster-domain=cluster.local \
  --kubeconfig=/var/lib/kubelet/kubeconfig \
  --network-plugin=kubenet \
  --non-masquerade-cidr=10.2.0.0/16 \
  --pod-cidr=10.2.1.0/24 \
  --register-node=true \
  --runtime-request-timeout=15m \
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
    user: system:node:worker-1
  name: default
current-context: "default"
kind: Config
preferences: {}
users:
- name: system:node:worker-1
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
$ sudo mv kubelet.service kube-proxy.service /etc/systemd/system/
$ sudo systemctl daemon-reload
$ sudo systemctl enable kubelet kube-proxy
$ sudo systemctl restart kubelet kube-proxy
```

Exit the ```worker-1``` instance SSH session to return to the installation system shell.

## ```worker-2``` Instance Creation

From the same installation system shell, create the ```worker-2``` instance
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
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=worker-2}]' \
  --private-ip-address 10.1.0.12 \
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
    --filter 'Name=tag:Name,Values=worker-2' \
    --query 'Reservations[].Instances[].InstanceId' \
    --output text)"
```

When the instance is running, SSH in.
```
$ ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=worker-2' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

## Installation and Configuration

Install Docker and Other Necessary Binaries
```
$ sudo apt-get update
$ sudo apt-get install docker.io socat conntrack --yes
```

Configure and Start Docker
```
$ echo "DOCKER_OPTS=--ip-masq=false --iptables=false \
--log-driver=json-file --log-level=warn \
--log-opt=max-file=5 --log-opt=max-size=10m \
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

Download the Kubernetes Binaries
```
$ export K8S_RELEASE="1.9.2"
$ wget -q --show-progress --https-only --timestamping \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-proxy" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kubelet"
```

Make the Kubernetes Binaries Executable and Place them in the PATH
```
$ chmod +x kube-proxy kubelet
$ sudo mv kube-proxy kubelet /usr/local/bin/
```

Configure the ```kubelet``` Systemd Unit
```
$ cat > kubelet.service <<EOF
[Unit]
Description=Kubernetes Kubelet
Documentation=https://github.com/kubernetes/kubernetes
[Service]
ExecStart=/usr/local/bin/kubelet \
  --allow-privileged=true \
  --cluster-dns=10.3.0.10 \
  --cluster-domain=cluster.local \
  --kubeconfig=/var/lib/kubelet/kubeconfig \
  --network-plugin=kubenet \
  --non-masquerade-cidr=10.2.0.0/16 \
  --pod-cidr=10.2.2.0/24 \
  --register-node=true \
  --runtime-request-timeout=15m \
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
    user: system:node:worker-2
  name: default
current-context: "default"
kind: Config
preferences: {}
users:
- name: system:node:worker-2
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
$ sudo mv kubelet.service kube-proxy.service /etc/systemd/system/
$ sudo systemctl daemon-reload
$ sudo systemctl enable kubelet kube-proxy
$ sudo systemctl restart kubelet kube-proxy
```

Exit the ```worker-2``` instance SSH session to return to the installation system shell.

## Routing the Pod CIDR for ```kubenet```

Obtain the Route Table ID
```
$ ROUTETABLE_ID=$(aws ec2 describe-route-tables \
  --region ${AWS_DEFAULT_REGION} \
  --filter "Name=tag:Name,Values=${STACK_NAME}-rt" \
  --query 'RouteTables[*].RouteTableId' \
  --output text)
```

Obtain the ```worker-1``` ENI ID
```
$ WORKER1ENI_ID=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=worker-1' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].NetworkInterfaceId' \
  --output text)
```

Add the ```worker-1``` Pod CIDR Route to the Route Table
```
$ aws ec2 create-route \
  --region ${AWS_DEFAULT_REGION} \
  --route-table-id ${ROUTETABLE_ID} \
  --network-interface-id ${WORKER1ENI_ID} \
  --destination-cidr-block '10.2.1.0/24' \
  --output text
```

Obtain the ```worker-2``` ENI ID
```
$ WORKER2ENI_ID=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=worker-2' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].NetworkInterfaceId' \
  --output text)
```

Add the ```worker-1``` Pod CIDR Route to the Route Table
```
$ aws ec2 create-route \
  --region ${AWS_DEFAULT_REGION} \
  --route-table-id ${ROUTETABLE_ID} \
  --network-interface-id ${WORKER2ENI_ID} \
  --destination-cidr-block '10.2.2.0/24' \
  --output text
```

## Validation

SSH into the master instance
```
$ ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=master' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

Ensure all the nodes have joined:
```
$ kubectl get nodes
NAME           STATUS    ROLES     AGE       VERSION
ip-10-1-0-10   Ready     <none>    4m        v1.9.2
ip-10-1-0-11   Ready     <none>    2m        v1.9.2
ip-10-1-0-12   Ready     <none>    46s       v1.9.2
```

[Back](/README.md) | [Next](create-kubeconfig.md)
