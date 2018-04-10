# Access the Kubernetes Worker

## Services Available

- `22/tcp` - [SSH](https://openssh.org)
- `4194/tcp` - [Kubelet cAdvisor endpoint](https://github.com/google/cadvisor)
- `10248/tcp` - [Kubelet Healthz Endpoint](https://kubernetes.io/docs/reference/generated/kubelet/)
- `10249/tcp` - [Kube-Proxy Metrics](https://kubernetes.io/docs/reference/generated/kube-proxy/)
- `10250/tcp` - [Kubelet Read/Write API](https://kubernetes.io/docs/reference/generated/kubelet)
- `10255/tcp` - [Kubelet Read-only API](https://kubernetes.io/docs/reference/generated/kubelet)
- `10256/tcp` - [Kube-Proxy health check server](https://kubernetes.io/docs/reference/generated/kube-proxy/)

Set the SSH Key, Region, and `worker-1` IP to variables:

```
$ export KEY_NAME="hkfs"
$ export AWS_DEFAULT_REGION="us-east-1"
$ export WORKER1IP=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=worker-1' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

### Probe the `ssh` port:

Verify the port is responding:
```
$ nc -v $WORKER1IP 22
Connection to 54.234.61.224 port 22 [tcp/ssh] succeeded!
SSH-2.0-OpenSSH_7.2p2 Ubuntu-4ubuntu2.4
```

### Probe the `cAdvisor` service:

Verify the port is responding:
```
$ $ nc -vz $WORKER1IP 4194
Connection to 54.234.61.224 port 4194 [tcp/*] succeeded!
```
Hit `tcp/4194` via curl:
```
$ curl $WORKER1IP:4194
<a href="/containers/">Temporary Redirect</a>.
```

Just like the [controller](direct-controller.md), the `worker` provides similar metrics from `cAdvisor` for this host.

### Probe the `Kubelet Healthz` service:

Verify the port is responding:
```
$ nc -vz $WORKER1IP 10248
nc: connect to 54.234.61.224 port 10248 (tcp) failed: Connection refused
```

It's only running on the `localhost` address:
```
$ ssh -i ${KEY_NAME}.pem ubuntu@$WORKER1IP
ubuntu@ip-10-1-0-11:~$ curl localhost:10248/
404 page not found
ubuntu@ip-10-1-0-11:~$ curl localhost:10248/healthz
ok
```

### Probe the `Kube-Proxy Metrics` service:

Verify the port is responding:
```
$ nc -vz $WORKER1IP 10249
nc: connect to 54.234.61.224 port 10249 (tcp) failed: Connection refused
```

It's only running on the `localhost` address:
```
$ ssh -i ${KEY_NAME}.pem ubuntu@$WORKER1IP
ubuntu@ip-10-1-0-11:~$ curl localhost:10249/healthz
ok
ubuntu@ip-10-1-0-11:~$ curl localhost:10249/metrics
```

Similar to the [controller](direct-controller.md), metrics on this host's `kube-proxy` are available at this endpoint.

### Probe the `Kubelet Read/Write` service:

Verify the port is responding:
```
$ nc -vz $WORKER1IP 10250
Connection to 54.234.61.224 port 10250 [tcp/*] succeeded!
```

Hit `tcp/10250` via curl:

```
$ curl -sk https://$WORKER1IP:10250/
```

So, the Kubelet is always listening on a TLS port, but by default, it's not authenticating or authorizing access to it.  The `-s` is to be "silent" and the `-k` tells curl to allow connections without certificates.

According to the source code, the following endpoints are available on both the Kubelet "read-only" API and "read/write" API:

- `/metrics`
- `/metrics/cadvisor`
- `/spec/`
- `/stats/`

The following endpoints are only available on the Kubelet's "read/write" API:
- `/logs/` - Get logs from a pod/container.
- `/run/` - Alias for `/exec/`
- `/exec/` - Exec a command in a running container
- `/attach/` - Attach to the `stdout` of a running container
- `/portForward/` - Forward a port directly to a container
- `/containerLogs/` - Get logs from a pod/container.
- `/runningpods/` - Lists all running pods in short JSON form
- `/debug/pprof/` - Various go debugging performance endpoints

Refer to the [controller](direct-controller.md) services details for example outputs as these provide similar output for that given node running the `kubelet`.

### Probe the `Kubelet Read-Only` service:

Verify the port is responding:
```
$ nc -vz $WORKER1IP 10255
Connection to 54.234.61.224 port 10255 [tcp/*] succeeded!
```
According to the source code, the following endpoints are available on the Kubelet "read-only" API:

- `/metrics`
- `/metrics/cadvisor`
- `/spec/`
- `/stats/`

Refer to the [controller](direct-controller.md) services details for example outputs as these provide similar output for that given node running the `kubelet`.

### Probe the `Kube-Proxy Healthcheck` service:

Verify the port is responding:
```
$ nc -vz $WORKER1IP 10256
Connection to 54.234.61.224 port 10256 [tcp/*] succeeded!
```
Hit `tcp/10256` via curl:
```
$ curl $WORKER1IP:10256
404 page not found
$ curl $WORKER1IP:10256/healthz
{"lastUpdated": "2018-03-27 21:17:09.14039841 +0000 UTC m=+888081.767665429","currentTime": "2018-03-27 21:17:36.679120214 +0000 UTC m=+888109.306387163"}
```

[Back](/README.md)
