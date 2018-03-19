# Launch and Configure the ```etcd``` Instance

## Instance Creation

From the same shell on installation system, create the ```etcd``` instance
```
$ aws ec2 run-instances \
  --region ${AWS_DEFAULT_REGION} \
  --image-id ${IMAGE_ID} \
  --count 1 \
  --instance-type t2.micro \
  --key-name ${KEY_NAME} \
  --subnet-id ${SUBNET_ID} \
  --associate-public-ip-address \
  --query 'Instances[0].InstanceId' \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=etcd}]' \
  --private-ip-address 10.1.0.5 \
  --block-device-mapping 'DeviceName=/dev/sda1,Ebs={VolumeSize=32}' \
  --output text
```

When the instance is running, SSH in.
```
$ ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=etcd' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

## Installation and Configuration
Download the ```etcd``` Binary
```
$ wget -q --show-progress --https-only --timestamping \
  "https://github.com/coreos/etcd/releases/download/v3.2.11/etcd-v3.2.11-linux-amd64.tar.gz"
```

Extract the ```etcd``` Binary and Create Needed Folders.
```
$ tar -xvf etcd-v3.2.11-linux-amd64.tar.gz
$ sudo mv etcd-v3.2.11-linux-amd64/etcd* /usr/local/bin/
$ sudo mkdir -p /etc/etcd /var/lib/etcd
```

Create the ```etcd.service``` Systemd Unit
```
$ cat > etcd.service <<EOF
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

Enable and Start/Restart ```etcd```
```
$ sudo mv etcd.service /etc/systemd/system/
$ sudo systemctl daemon-reload
$ sudo systemctl enable etcd
$ sudo systemctl restart etcd
```

## Validation
Verify ```etcd``` is Running Properly
```
$ ETCDCTL_API=3 etcdctl member list
b8ae04a310fbeaf8, started, ip-10-10-10-5, http://10.1.0.5:2380, http://10.1.0.5:2379
```
Exit the ```etcd``` instance SSH session to return to the installation system shell.

[Back](/README.md) | [Next](launch-configure-master.md)
