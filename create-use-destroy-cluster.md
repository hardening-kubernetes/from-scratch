# Installation
## Create the VPC

### Cloudformation

#### Deploy the Cloudformation Template

```shell
export STACK_NAME="hkfs"
export AWS_DEFAULT_REGION="us-east-1"
export KEY_NAME="hkfs"
export IMAGE_ID="ami-66506c1c"

aws cloudformation create-stack --region ${AWS_DEFAULT_REGION} --stack-name ${STACK_NAME} --template-body file://${STACK_NAME}.json --output text

```

#### Obtain the Necessary Variables

```shell
VPC_ID="$(aws cloudformation describe-stacks --region ${AWS_DEFAULT_REGION} --query 'Stacks[*].Outputs[?OutputKey==`VPCId`].OutputValue[]' --stack-name ${STACK_NAME} --output text)"
echo "${VPC_ID}"
SG_ID="$(aws ec2 describe-security-groups --query 'SecurityGroups[*].GroupId' --region ${AWS_DEFAULT_REGION} --filter "Name=vpc-id,Values=${VPC_ID}" --output text)"
echo "${SG_ID}"
SUBNET_ID="$(aws ec2 describe-subnets --region ${AWS_DEFAULT_REGION} --filter "Name=tag:Name,Values=${STACK_NAME}-subnet" --query 'Subnets[*].SubnetId' --output text)"
echo "${SUBNET_ID}"
```

#### Allow Just our IPs to Reach these Instances on all Ports

```shell
aws ec2 authorize-security-group-ingress --region ${AWS_DEFAULT_REGION} --group-id ${SG_ID} --protocol all --port 0 --cidr $(dig +short myip.opendns.com @resolver1.opendns.com)/32
```

## Instance Creation

### Prepare a Keypair

#### Create an SSH Keypair

```shell
aws ec2 create-key-pair --region ${AWS_DEFAULT_REGION} --key-name ${KEY_NAME} --query 'KeyMaterial' --output text > ${KEY_NAME}.pem
chmod 600 ${KEY_NAME}.pem
```

### Create Systems

#### Etcd

```shell
aws ec2 run-instances --region ${AWS_DEFAULT_REGION} --image-id ${IMAGE_ID} --count 1 --instance-type t2.micro --key-name ${KEY_NAME} --subnet-id ${SUBNET_ID} --associate-public-ip-address --query 'Instances[0].InstanceId' --output text --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=etcd}]' --private-ip-address 10.1.0.5 --block-device-mapping 'DeviceName=/dev/sda1,Ebs={VolumeSize=32}'
```

#### Master

```shell
aws ec2 run-instances --region ${AWS_DEFAULT_REGION} --image-id ${IMAGE_ID} --count 1 --instance-type t2.small --key-name ${KEY_NAME} --subnet-id ${SUBNET_ID} --associate-public-ip-address --query 'Instances[0].InstanceId' --output text --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=master}]' --private-ip-address 10.1.0.10 --block-device-mapping 'DeviceName=/dev/sda1,Ebs={VolumeSize=32}'
```

#### Worker-1 and Worker-2

```shell
aws ec2 run-instances --region ${AWS_DEFAULT_REGION} --image-id ${IMAGE_ID} --count 1 --instance-type t2.small --key-name ${KEY_NAME} --subnet-id ${SUBNET_ID} --associate-public-ip-address --query 'Instances[0].InstanceId' --output text --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=worker-1}]' --private-ip-address 10.1.0.11 --block-device-mapping 'DeviceName=/dev/sda1,Ebs={VolumeSize=32}'

aws ec2 run-instances --region ${AWS_DEFAULT_REGION} --image-id ${IMAGE_ID} --count 1 --instance-type t2.small --key-name ${KEY_NAME} --subnet-id ${SUBNET_ID} --associate-public-ip-address --query 'Instances[0].InstanceId' --output text --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=worker-2}]' --private-ip-address 10.1.0.12 --block-device-mapping 'DeviceName=/dev/sda1,Ebs={VolumeSize=32}'
```

## VPC Routing

### Per-Node Pod CIDR Routes

#### Obtain the Route Table ID

```shell
ROUTETABLE_ID=$(aws ec2 describe-route-tables --region ${AWS_DEFAULT_REGION} --filter "Name=tag:Name,Values=${STACK_NAME}-rt" --query 'RouteTables[*].RouteTableId' --output text)
```

#### Obtain the ```master``` ENI ID

```shell
MASTERENI_ID=$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=master' --query 'Reservations[].Instances[].NetworkInterfaces[0].NetworkInterfaceId' --output text)
```

#### Add the ```master``` Pod CIDR Route to the Route Table

```shell
aws ec2 create-route --region ${AWS_DEFAULT_REGION} --route-table-id ${ROUTETABLE_ID} --network-interface-id ${MASTERENI_ID} --destination-cidr-block '10.2.0.0/24' --output text
```

#### Obtain the ```worker-1``` ENI ID

```shell
WORKER1ENI_ID=$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=worker-1' --query 'Reservations[].Instances[].NetworkInterfaces[0].NetworkInterfaceId' --output text)
```

#### Add the ```worker-1``` Pod CIDR Route to the Route Table

```shell
aws ec2 create-route --region ${AWS_DEFAULT_REGION} --route-table-id ${ROUTETABLE_ID} --network-interface-id ${WORKER1ENI_ID} --destination-cidr-block '10.2.1.0/24' --output text
```

#### Obtain the ```worker-2``` ENI ID

```shell
WORKER2ENI_ID=$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=worker-2' --query 'Reservations[].Instances[].NetworkInterfaces[0].NetworkInterfaceId' --output text)
```

#### Add the ```worker-2``` Pod CIDR Route to the Route Table

```shell
aws ec2 create-route --region ${AWS_DEFAULT_REGION} --route-table-id ${ROUTETABLE_ID} --network-interface-id ${WORKER2ENI_ID} --destination-cidr-block '10.2.2.0/24' --output text
```

## Instance Configuration

### Configure the Etcd Instance

#### SSH Into the Etcd Instance
```shell
ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=etcd' --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' --output text)
```

#### Download the Etcd Binary
```shell
wget -q --show-progress --https-only --timestamping \
  "https://github.com/coreos/etcd/releases/download/v3.2.11/etcd-v3.2.11-linux-amd64.tar.gz"
```

#### Extract the Etcd Binary and Create Needed Folders
```shell
tar -xvf etcd-v3.2.11-linux-amd64.tar.gz
sudo mv etcd-v3.2.11-linux-amd64/etcd* /usr/local/bin/
sudo mkdir -p /etc/etcd /var/lib/etcd
```

#### Create the ```etcd.service``` Systemd Unit
```shell
cat > etcd.service <<EOF
[Unit]
Description=etcd
Documentation=https://github.com/coreos
[Service]
ExecStart=/usr/local/bin/etcd \\
  --name ip-10-1-0-5 \\
  --initial-advertise-peer-urls http://10.1.0.5:2380 \\
  --listen-peer-urls http://10.1.0.5:2380 \\
  --listen-client-urls http://10.1.0.5:2379,http://127.0.0.1:2379 \\
  --advertise-client-urls http://10.1.0.5:2379 \\
  --initial-cluster-token etcd-cluster-0 \\
  --initial-cluster ip-10-1-0-5=http://10.1.0.5:2380 \\
  --initial-cluster-state new \\
  --data-dir=/var/lib/etcd
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
EOF
```

#### Enable and Start/Restart Etcd
```shell
sudo mv etcd.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable etcd
sudo systemctl restart etcd
```

#### Verify Etcd is Running Properly
```shell
ETCDCTL_API=3 etcdctl member list
b8ae04a310fbeaf8, started, ip-10-10-10-5, http://10.1.0.5:2380, http://10.1.0.5:2379
```

### Configure the ```master``` Instance

#### Disable Source/Destination Checking for ```kube-proxy```

```shell
aws ec2 modify-instance-attribute --region ${AWS_DEFAULT_REGION} --no-source-dest-check --instance-id "$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=master' --query 'Reservations[].Instances[].InstanceId' --output text)"
```

#### SSH Into the ```master``` Instance

```shell
ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=master' --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' --output text)
```

#### Install Docker and Other Necessary Binaries
```shell
sudo apt-get update
sudo apt-get install docker.io socat conntrack --yes
```

#### Configure and Start Docker
```shell
echo "DOCKER_OPTS=--ip-masq=false --iptables=false --log-driver=json-file --log-level=warn --log-opt=max-file=5 --log-opt=max-size=10m --storage-driver=overlay" | sudo tee -a /etc/default/docker
sudo systemctl daemon-reload
sudo systemctl restart docker.service
```

#### Verify Docker is Running
```shell
sudo docker ps
```

#### Enable Forwarding for ```kube-proxy``` Functions
```shell
sudo iptables -P FORWARD ACCEPT
```

#### Download the Kubernetes Binaries
```shell
export K8S_RELEASE="1.9.2"
wget -q --show-progress --https-only --timestamping \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-apiserver" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-controller-manager" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-scheduler" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kubectl" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-proxy" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kubelet"
```

#### Make the Kubernetes Binaries Executable and Place them in the PATH
```shell
chmod +x kube-apiserver kube-controller-manager kube-scheduler kubectl kubelet kube-proxy
sudo mv kube-apiserver kube-controller-manager kube-scheduler kubectl kubelet kube-proxy /usr/local/bin/
```

#### Create Supporting Directories
```shell
sudo mkdir -p \
  /etc/cni/net.d \
  /opt/cni/bin \
  /var/lib/kubelet \
  /var/lib/kube-proxy \
  /var/lib/kubernetes \
  /var/run/kubernetes
```

#### Download the CNI Plugins for ```kubenet```
```shell
wget -q --show-progress --https-only --timestamping \
  "https://github.com/containernetworking/plugins/releases/download/v0.6.0/cni-plugins-amd64-v0.6.0.tgz"
```

#### Install the CNI Plugins
```shell
sudo tar -xvf cni-plugins-amd64-v0.6.0.tgz -C /opt/cni/bin/
```

#### Configure the ```kube-apiserver``` Systemd Unit
```shell
cat > kube-apiserver.service <<EOF
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

#### Configure the ```kube-controller-manager``` Systemd Unit
```shell
cat > kube-controller-manager.service <<EOF
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

#### Configure the ```kube-scheduler``` Systemd Unit
```shell
cat > kube-scheduler.service <<EOF
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

#### Configure the ```kubelet``` Systemd Unit
```shell
cat > kubelet.service <<EOF
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

#### Create the ```kubelet``` ```kubeconfig``` File
```shell
cat > kubeconfig <<EOF
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

#### Configure the ```kube-proxy``` Systemd Unit
```shell
cat > kube-proxy.service <<EOF
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

#### Place the Systemd Units and Start All Services
```shell
sudo mv kubeconfig /var/lib/kubelet/kubeconfig
sudo mv kube-apiserver.service kube-scheduler.service kube-controller-manager.service kubelet.service kube-proxy.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable kube-apiserver kube-controller-manager kube-scheduler kubelet kube-proxy
sudo systemctl restart kube-apiserver kube-controller-manager kube-scheduler kubelet kube-proxy
```

#### Verify that the Cluster Components are Running and Healthy
```shell
kubectl get componentstatuses
```
##### Example of Healthy Output
```shell
NAME                 STATUS    MESSAGE              ERROR
controller-manager   Healthy   ok                   
scheduler            Healthy   ok                   
etcd-0               Healthy   {"health": "true"} 
```

### Configure the ```worker-1``` Instance

#### Disable Source/Destination Checking for ```kube-proxy```

```shell
aws ec2 modify-instance-attribute --region ${AWS_DEFAULT_REGION} --no-source-dest-check --instance-id "$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=worker-1' --query 'Reservations[].Instances[].InstanceId' --output text)"
```

#### SSH Into the ```worker-1``` Instance
```shell
ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=worker-1' --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' --output text)
```
#### Install Docker and Other Necessary Binaries
```shell
sudo apt-get update
sudo apt-get install docker.io socat conntrack --yes
```

#### Configure and Start Docker
```shell
echo "DOCKER_OPTS=--ip-masq=false --iptables=false --log-driver=json-file --log-level=warn --log-opt=max-file=5 --log-opt=max-size=10m --storage-driver=overlay" | sudo tee -a /etc/default/docker
sudo systemctl daemon-reload
sudo systemctl restart docker.service
```

#### Verify Docker is Running
```shell
sudo docker ps
```

#### Enable Forwarding for ```kube-proxy``` Functions
```shell
sudo iptables -P FORWARD ACCEPT
```

#### Create Supporting Directories
```shell
sudo mkdir -p \
  /etc/cni/net.d \
  /opt/cni/bin \
  /var/lib/kubelet \
  /var/lib/kube-proxy \
  /var/lib/kubernetes \
  /var/run/kubernetes
```

#### Download the CNI Plugins for ```kubenet```
```shell
wget -q --show-progress --https-only --timestamping \
  "https://github.com/containernetworking/plugins/releases/download/v0.6.0/cni-plugins-amd64-v0.6.0.tgz"
```

#### Install the CNI Plugins
```shell
sudo tar -xvf cni-plugins-amd64-v0.6.0.tgz -C /opt/cni/bin/
```

#### Download the Kubernetes Binaries
```shell
export K8S_RELEASE="1.9.2"
wget -q --show-progress --https-only --timestamping \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-proxy" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kubelet"
```

#### Make the Kubernetes Binaries Executable and Place them in the PATH
```shell
chmod +x kube-proxy kubelet
sudo mv kube-proxy kubelet /usr/local/bin/
```

#### Configure the ```kubelet``` Systemd Unit
```shell
cat > kubelet.service <<EOF
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

#### Create the ```kubelet``` ```kubeconfig``` File
```shell
cat > kubeconfig <<EOF
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

#### Configure the ```kube-proxy``` Systemd Unit
```shell
cat > kube-proxy.service <<EOF
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

#### Place the Systemd Units and Start All Services
```shell
sudo mv kubeconfig /var/lib/kubelet/kubeconfig
sudo mv kubelet.service kube-proxy.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable kubelet kube-proxy
sudo systemctl restart kubelet kube-proxy
```

### Configure the ```worker-2``` Instance

#### Disable Source/Destination Checking for ```kube-proxy```

```shell
aws ec2 modify-instance-attribute --region ${AWS_DEFAULT_REGION} --no-source-dest-check --instance-id "$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=worker-2' --query 'Reservations[].Instances[].InstanceId' --output text)"
```

#### SSH Into the ```worker-2``` Instance
```shell
ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=worker-2' --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' --output text)
```

#### Install Docker and Other Necessary Binaries
```shell
sudo apt-get update
sudo apt-get install docker.io socat conntrack --yes
```

#### Configure and Start Docker
```shell
echo "DOCKER_OPTS=--ip-masq=false --iptables=false --log-driver=json-file --log-level=warn --log-opt=max-file=5 --log-opt=max-size=10m --storage-driver=overlay" | sudo tee -a /etc/default/docker
sudo systemctl daemon-reload
sudo systemctl restart docker.service
```

#### Verify Docker is Running
```shell
sudo docker ps
```

#### Enable Forwarding for ```kube-proxy``` Functions
```shell
sudo iptables -P FORWARD ACCEPT
```

#### Create Supporting Directories
```shell
sudo mkdir -p \
  /etc/cni/net.d \
  /opt/cni/bin \
  /var/lib/kubelet \
  /var/lib/kube-proxy \
  /var/lib/kubernetes \
  /var/run/kubernetes
```

#### Download the CNI Plugins for ```kubenet```
```shell
wget -q --show-progress --https-only --timestamping \
  "https://github.com/containernetworking/plugins/releases/download/v0.6.0/cni-plugins-amd64-v0.6.0.tgz"
```

#### Install the CNI Plugins
```shell
sudo tar -xvf cni-plugins-amd64-v0.6.0.tgz -C /opt/cni/bin/
```

#### Download the Kubernetes Binaries
```shell
export K8S_RELEASE="1.9.2"
wget -q --show-progress --https-only --timestamping \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kube-proxy" \
  "https://storage.googleapis.com/kubernetes-release/release/v${K8S_RELEASE}/bin/linux/amd64/kubelet"
```

#### Make the Kubernetes Binaries Executable and Place them in the PATH
```shell
chmod +x kube-proxy kubelet
sudo mv kube-proxy kubelet /usr/local/bin/
```

#### Configure the ```kubelet``` Systemd Unit
```shell
cat > kubelet.service <<EOF
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

#### Create the ```kubelet``` ```kubeconfig``` File
```shell
cat > kubeconfig <<EOF
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

#### Configure the ```kube-proxy``` Systemd Unit
```shell
cat > kube-proxy.service <<EOF
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

#### Place the Systemd Units and Start All Services
```shell
sudo mv kubeconfig /var/lib/kubelet/kubeconfig
sudo mv kubelet.service kube-proxy.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable kubelet kube-proxy
sudo systemctl restart kubelet kube-proxy
```

## Verify via ```kubectl``` on the ```master```

```shell
ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=master' --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' --output text)
```

#### Ensure the Nodes have Joined

```shell
kubectl get nodes
NAME           STATUS    ROLES     AGE       VERSION
ip-10-1-0-10   Ready     <none>    4m        v1.9.2
ip-10-1-0-11   Ready     <none>    2m        v1.9.2
ip-10-1-0-12   Ready     <none>    46s       v1.9.2
```

#### Install ```kube-dns```

##### Create the ```kube-dns.yml``` Definition

```shell
cat > kube-dns.yml <<EOF
# Copyright 2016 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

apiVersion: v1
kind: Service
metadata:
  name: kube-dns
  namespace: kube-system
  labels:
    k8s-app: kube-dns
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
    kubernetes.io/name: "KubeDNS"
spec:
  selector:
    k8s-app: kube-dns
  clusterIP: 10.3.0.10
  ports:
  - name: dns
    port: 53
    protocol: UDP
  - name: dns-tcp
    port: 53
    protocol: TCP
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: kube-dns
  namespace: kube-system
  labels:
    addonmanager.kubernetes.io/mode: EnsureExists
---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: kube-dns
  namespace: kube-system
  labels:
    k8s-app: kube-dns
    kubernetes.io/cluster-service: "true"
    addonmanager.kubernetes.io/mode: Reconcile
spec:
  # replicas: not specified here:
  # 1. In order to make Addon Manager do not reconcile this replicas parameter.
  # 2. Default is 1.
  # 3. Will be tuned in real time if DNS horizontal auto-scaling is turned on.
  strategy:
    rollingUpdate:
      maxSurge: 10%
      maxUnavailable: 0
  selector:
    matchLabels:
      k8s-app: kube-dns
  template:
    metadata:
      labels:
        k8s-app: kube-dns
      annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ''
    spec:
      tolerations:
      - key: "CriticalAddonsOnly"
        operator: "Exists"
      volumes:
      - name: kube-dns-config
        configMap:
          name: kube-dns
          optional: true
      containers:
      - name: kubedns
        image: gcr.io/google_containers/k8s-dns-kube-dns-amd64:1.14.8
        resources:
          # TODO: Set memory limits when we've profiled the container for large
          # clusters, then set request = limit to keep this container in
          # guaranteed class. Currently, this container falls into the
          # "burstable" category so the kubelet doesn't backoff from restarting it.
          limits:
            memory: 170Mi
          requests:
            cpu: 100m
            memory: 70Mi
        livenessProbe:
          httpGet:
            path: /healthcheck/kubedns
            port: 10054
            scheme: HTTP
          initialDelaySeconds: 60
          timeoutSeconds: 5
          successThreshold: 1
          failureThreshold: 5
        readinessProbe:
          httpGet:
            path: /readiness
            port: 8081
            scheme: HTTP
          # we poll on pod startup for the Kubernetes master service and
          # only setup the /readiness HTTP server once that's available.
          initialDelaySeconds: 3
          timeoutSeconds: 5
        args:
        - --domain=cluster.local.
        - --kube-master-url=http://10.1.0.10:8080
        - --dns-port=10053
        - --config-dir=/kube-dns-config
        - --v=2
        env:
        - name: PROMETHEUS_PORT
          value: "10055"
        ports:
        - containerPort: 10053
          name: dns-local
          protocol: UDP
        - containerPort: 10053
          name: dns-tcp-local
          protocol: TCP
        - containerPort: 10055
          name: metrics
          protocol: TCP
        volumeMounts:
        - name: kube-dns-config
          mountPath: /kube-dns-config
      - name: dnsmasq
        image: gcr.io/google_containers/k8s-dns-dnsmasq-nanny-amd64:1.14.8
        livenessProbe:
          httpGet:
            path: /healthcheck/dnsmasq
            port: 10054
            scheme: HTTP
          initialDelaySeconds: 60
          timeoutSeconds: 5
          successThreshold: 1
          failureThreshold: 5
        args:
        - -v=2
        - -logtostderr
        - -configDir=/etc/k8s/dns/dnsmasq-nanny
        - -restartDnsmasq=true
        - --
        - -k
        - --cache-size=1000
        - --no-negcache
        - --log-facility=-
        - --server=/cluster.local/127.0.0.1#10053
        - --server=/in-addr.arpa/127.0.0.1#10053
        - --server=/ip6.arpa/127.0.0.1#10053
        ports:
        - containerPort: 53
          name: dns
          protocol: UDP
        - containerPort: 53
          name: dns-tcp
          protocol: TCP
        # see: https://github.com/kubernetes/kubernetes/issues/29055 for details
        resources:
          requests:
            cpu: 150m
            memory: 20Mi
        volumeMounts:
        - name: kube-dns-config
          mountPath: /etc/k8s/dns/dnsmasq-nanny
      - name: sidecar
        image: gcr.io/google_containers/k8s-dns-sidecar-amd64:1.14.8
        livenessProbe:
          httpGet:
            path: /metrics
            port: 10054
            scheme: HTTP
          initialDelaySeconds: 60
          timeoutSeconds: 5
          successThreshold: 1
          failureThreshold: 5
        args:
        - --v=2
        - --logtostderr
        - --probe=kubedns,127.0.0.1:10053,kubernetes.default.svc.cluster.local,5,SRV
        - --probe=dnsmasq,127.0.0.1:53,kubernetes.default.svc.cluster.local,5,SRV
        ports:
        - containerPort: 10054
          name: metrics
          protocol: TCP
        resources:
          requests:
            memory: 20Mi
            cpu: 10m
      dnsPolicy: Default  # Don't use cluster DNS.
EOF
```

##### Deploy ```kube-dns```

```shell
kubectl create -f kube-dns.yml
```


# Using the Cluster

## Deploying a Sample Application
#### Obtain the ```master``` External IP
```shell
API_IP=$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=master' --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' --output text)
```

#### Create a Local ```kubeconfig``` File
```shell
cat > kubeconfig <<EOF
apiVersion: v1
clusters:
- cluster:
    server: http://${API_IP}:8080
  name: default
contexts:
- context:
    cluster: default
  name: default
current-context: "default"
kind: Config
preferences: {}
EOF
```

#### Create an ```nginx``` Test Pod
```shell
export KUBECONFIG=./kubeconfig
kubectl get pods --all-namespaces
kubectl run nginx --image=nginx
kubectl get pods -l run=nginx
```

#### Obtain the Pod Name
```shell
POD_NAME=$(kubectl get pods -l run=nginx -o jsonpath="{.items[0].metadata.name}")
```

#### Verify Port Forwarding
```shell
kubectl port-forward $POD_NAME 8080:80
curl localhost:8080
```

#### Verify Pod Logging
```shell
kubectl logs $POD_NAME -f
127.0.0.1 - - [03/Feb/2018:05:45:46 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.54.0" "-"
```

#### Verify Pod Exec
```shell
kubectl exec -ti $POD_NAME -- nginx -v
nginx version: nginx/1.13.8
```

#### Verify Exposed Service

```shell
NODE_IP=$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=worker-1' --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' --output text)
kubectl expose deployment nginx --port 80 --type NodePort
NODE_PORT=$(kubectl get svc nginx --output=jsonpath='{range .spec.ports[0]}{.nodePort}')
curl ${NODE_IP}:${NODE_PORT}
```

# Cleanup
## Instance Deletion
### ```worker-1``` and ```worker-2``` Deletion
#### Obtain the Worker Instance IDs
```shell
export AWS_DEFAULT_REGION="us-east-1"

INSTANCE1_ID="$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=worker-1' --query 'Reservations[].Instances[].InstanceId' --output text)"
INSTANCE2_ID="$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=worker-2' --query 'Reservations[].Instances[].InstanceId' --output text)"
```

#### Terminate ```worker-1``` and ```worker-2```
```shell
aws ec2 terminate-instances --region ${AWS_DEFAULT_REGION} --instance-ids ${INSTANCE1_ID}
aws ec2 terminate-instances --region ${AWS_DEFAULT_REGION} --instance-ids ${INSTANCE2_ID}
```

### ```master``` Deletion

#### Obtain the ```master``` Instance ID and Terminate
```shell
INSTANCEM_ID="$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=master' --query 'Reservations[].Instances[].InstanceId' --output text)"
aws ec2 terminate-instances --region ${AWS_DEFAULT_REGION} --instance-ids ${INSTANCEM_ID}
```

### ```etcd``` Deletion

#### Obtain the ```etcd``` Instance ID and Terminate

```shell
INSTANCEE_ID="$(aws ec2 describe-instances --region ${AWS_DEFAULT_REGION} --filter 'Name=tag:Name,Values=etcd' --query 'Reservations[].Instances[].InstanceId' --output text)"
aws ec2 terminate-instances --region ${AWS_DEFAULT_REGION} --instance-ids ${INSTANCEE_ID}
```
### SSH Keypair Deletion
```shell
aws ec2 delete-key-pair --region ${AWS_DEFAULT_REGION} --key-name "${KEY_NAME}"
```

## Cloudformation/VPC Deletion

#### Delete the Stack
```shell
aws cloudformation delete-stack --region ${AWS_DEFAULT_REGION} --stack-name ${STACK_NAME}
```