# Enumerate Exposed Ports and Services

## Etcd

SSH into the `etcd` instance
```
$ export KEY_NAME="hkfs"
$ export AWS_DEFAULT_REGION="us-east-1"
$ ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=etcd' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```
On the `etcd` system, list the services and processes running:
```
$ sudo lsof -i | grep LIST
etcd      1112   root    5u  IPv4  15682      0t0  TCP ip-10-1-0-5.ec2.internal:2380 (LISTEN)
etcd      1112   root    6u  IPv4  15683      0t0  TCP ip-10-1-0-5.ec2.internal:2379 (LISTEN)
etcd      1112   root    7u  IPv4  15684      0t0  TCP localhost:2379 (LISTEN)
sshd      1137   root    3u  IPv4  15652      0t0  TCP *:ssh (LISTEN)
sshd      1137   root    4u  IPv6  15660      0t0  TCP *:ssh (LISTEN)
```
#### Services
- `22/tcp` - [SSH](https://openssh.org)
- `2379/tcp` - [Etcd server service](https://github.com/coreos/etcd#etcd-tcp-ports)
- `2380/tcp` - [Etcd peer discovery service](https://github.com/coreos/etcd#etcd-tcp-ports)

## Controller

SSH into the `controller` instance
```
$ ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=controller' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

On the `controller` system, list the services and processes running:
```
$ sudo lsof -i -nP | grep LIST
kube-prox  1237   root    6u  IPv6  16597      0t0  TCP *:10256 (LISTEN)
kube-prox  1237   root    8u  IPv4  16599      0t0  TCP 127.0.0.1:10249 (LISTEN)
kubelet    1257   root    6u  IPv6  17074      0t0  TCP *:4194 (LISTEN)
kubelet    1257   root   18u  IPv4  17213      0t0  TCP 127.0.0.1:10248 (LISTEN)
kubelet    1257   root   20u  IPv6  17216      0t0  TCP *:10250 (LISTEN)
kubelet    1257   root   21u  IPv6  17218      0t0  TCP *:10255 (LISTEN)
sshd       1268   root    3u  IPv4  15100      0t0  TCP *:22 (LISTEN)
sshd       1268   root    4u  IPv6  15102      0t0  TCP *:22 (LISTEN)
kube-cont  1275   root    5u  IPv6  16701      0t0  TCP *:10252 (LISTEN)
kube-apis  1297   root   67u  IPv6  19320      0t0  TCP *:8080 (LISTEN)
kube-sche  1303   root    3u  IPv6  16637      0t0  TCP *:10251 (LISTEN)
```

#### Services
- `22/tcp` - [SSH](https://openssh.org)
- `4194/tcp` - [Kubelet cAdvisor endpoint](https://github.com/google/cadvisor)
- `8080/tcp` - [Kubernetes API Server "Insecure" Port](https://kubernetes.io/docs/reference/generated/kube-apiserver/)
- `10248/tcp` - [Kubelet Healthz Endpoint](https://kubernetes.io/docs/reference/generated/kubelet/)
- `10249/tcp` - [Kube-Proxy Metrics](https://kubernetes.io/docs/reference/generated/kube-proxy/)
- `10250/tcp` - [Kubelet Read/Write API](https://kubernetes.io/docs/reference/generated/kubelet)
- `10251/tcp` - [Kubernetes Scheduler HTTP Service](https://kubernetes.io/docs/reference/generated/kube-scheduler/)
- `10252/tcp` - [Kubernetes Controller Manager HTTP Service](https://kubernetes.io/docs/reference/generated/kube-controller-manager/)
- `10255/tcp` - [Kubelet Read-only API](https://kubernetes.io/docs/reference/generated/kubelet)
- `10256/tcp` - [Kube-Proxy health check server](https://kubernetes.io/docs/reference/generated/kube-proxy/)

## Worker

SSH into the `worker-1` instance
```
$ ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=worker-1' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

On the `worker-1` system, list the services and processes running:
```
$ sudo lsof -i -nP | grep LIST
kube-prox  1240   root   10u  IPv4  16479      0t0  TCP 127.0.0.1:10249 (LISTEN)
kube-prox  1240   root   11u  IPv6  16484      0t0  TCP *:10256 (LISTEN)
kubelet    1251   root    6u  IPv6  16841      0t0  TCP *:4194 (LISTEN)
kubelet    1251   root   18u  IPv4  16945      0t0  TCP 127.0.0.1:10248 (LISTEN)
kubelet    1251   root   21u  IPv6  16950      0t0  TCP *:10250 (LISTEN)
kubelet    1251   root   22u  IPv6  16952      0t0  TCP *:10255 (LISTEN)
sshd       1299   root    3u  IPv4  15256      0t0  TCP *:22 (LISTEN)
sshd       1299   root    4u  IPv6  15258      0t0  TCP *:22 (LISTEN)
```

#### Services
- `22/tcp` - [SSH](https://openssh.org)
- `4194/tcp` - [Kubelet cAdvisor endpoint](https://github.com/google/cadvisor)
- `10248/tcp` - [Kubelet Healthz Endpoint](https://kubernetes.io/docs/reference/generated/kubelet/)
- `10249/tcp` - [Kube-Proxy Metrics](https://kubernetes.io/docs/reference/generated/kube-proxy/)
- `10250/tcp` - [Kubelet Read/Write API](https://kubernetes.io/docs/reference/generated/kubelet)
- `10255/tcp` - [Kubelet Read-only API](https://kubernetes.io/docs/reference/generated/kubelet)
- `10256/tcp` - [Kube-Proxy health check server](https://kubernetes.io/docs/reference/generated/kube-proxy/)

[Back](/README.md#level-0-attacks) | [Next](direct-etcd.md)
