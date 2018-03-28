# Access the Controller Services Directly

## Services Available

- `22/tcp` - [SSH](https://openssh.org)
- `4194/tcp` - [Kubelet cAdvisor endpoint](https://github.com/google/cadvisor)
- `6443/tcp` - [Kubernetes API Server "Secure" Port](https://kubernetes.io/docs/reference/generated/kube-apiserver/)
- `8080/tcp` - [Kubernetes API Server "Insecure" Port](https://kubernetes.io/docs/reference/generated/kube-apiserver/)
- `10248/tcp` - [Kubelet Healthz Endpoint](https://kubernetes.io/docs/reference/generated/kubelet/)
- `10249/tcp` - [Kube-Proxy Metrics](https://kubernetes.io/docs/reference/generated/kube-proxy/)
- `10250/tcp` - [Kubelet Read/Write API](https://kubernetes.io/docs/reference/generated/kubelet)
- `10251/tcp` - [Kubernetes Scheduler HTTP Service](https://kubernetes.io/docs/reference/generated/kube-scheduler/)
- `10252/tcp` - [Kubernetes Controller Manager HTTP Service](https://kubernetes.io/docs/reference/generated/kube-controller-manager/)
- `10255/tcp` - [Kubelet Read-only API](https://kubernetes.io/docs/reference/generated/kubelet)
- `10256/tcp` - [Kube-Proxy health check server](https://kubernetes.io/docs/reference/generated/kube-proxy/)

Set the `controller` IP to a variable:

```
$ export CONTROLLERIP=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=controller' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

### Probe the `ssh` port:

Verify the port is responding:
```
$ nc -v $CONTROLLERIP 22
Connection to 54.89.108.72 port 22 [tcp/ssh] succeeded!
SSH-2.0-OpenSSH_7.2p2 Ubuntu-4ubuntu2.4
```

### Probe the `cAdvisor` service:

Verify the port is responding:
```
$ $ nc -vz $CONTROLLERIP 4194
Connection to 54.89.108.72 port 4194 [tcp/*] succeeded!
```
Hit `tcp/4194` via curl:
```
$ curl $CONTROLLERIP:4194
<a href="/containers/">Temporary Redirect</a>.
```
Navigate to it using a browser:
```
$ open http://$CONTROLLERIP:4194/containers/
```
Hit `tcp/4194` on the Prometheus formatted `/metrics` endpoint:

 [```$ curl $CONTROLLERIP:4194/metrics```](cmd/cadvisor-metrics.md)

As you can see, there are many pieces of information that describe the infrastructure that should not be exposed to an attacker.

From the `/containers/` endpoint:

- How many CPU shares and how much RAM is available on this node.
- How much CPU/RAM/Disk/FS is in use, and that usage over time.
- A full process listing complete with base process name, PID, UID, GID, CPU usage, Memory usage, and time running.

From the `/metrics` endpoint:

- Useful information about the Host OS and container runtime:

  ```
  cadvisor_version_info{cadvisorRevision="",cadvisorVersion="",dockerVersion="1.13.1",kernelVersion="4.4.0-1049-aws",osVersion="Ubuntu 16.04.3 LTS"} 1
  ```

- When a container was created/started.  In this case: `Saturday, March 17, 2018 2:36:01 PM`

  ```
  container_start_time_seconds{container_name="kubernetes-dashboard",id="/kubepods/podd60f089f-0a85-11e8-9462-06d7638bd978/a3c7cd6d8e0ebf603b431604ab7a1844f8ed0b651b231b10849010fd7cf37b17",image="gcr.io/google_containers/kubernetes-dashboard-amd64@sha256:2c4421ed80358a0ee97b44357b6cd6dc09be6ccc27dfe9d50c9bfc39a760e5fe",name="k8s_kubernetes-dashboard_kubernetes-dashboard-5b575fd4c-77fqr_kube-system_d60f089f-0a85-11e8-9462-06d7638bd978_3",namespace="kube-system",pod_name="kubernetes-dashboard-5b575fd4c-77fqr"} 1.521297361e+09
  ```


- A full listing of the containers running on this host plus a lot of metadata about each one.  For example, this single metric from one pod offers:

  ```
  ...snip...
  container_cpu_load_average_10s{container_name="kubernetes-dashboard",id="/kubepods/podd60f089f-0a85-11e8-9462-06d7638bd978/a3c7cd6d8e0ebf603b431604ab7a1844f8ed0b651b231b10849010fd7cf37b17",image="gcr.io/google_containers/kubernetes-dashboard-amd64@sha256:2c4421ed80358a0ee97b44357b6cd6dc09be6ccc27dfe9d50c9bfc39a760e5fe",name="k8s_kubernetes-dashboard_kubernetes-dashboard-5b575fd4c-77fqr_kube-system_d60f089f-0a85-11e8-9462-06d7638bd978_3",namespace="kube-system",pod_name="kubernetes-dashboard-5b575fd4c-77fqr"} 0
  ...snip...
  ```

  - Container name: `kubernetes-dashboard`
  - Namespace: `kube-system`
  - Image: `gcr.io/google_containers/kubernetes-dashboard-amd64`
  - Image Version/Hash: `sha256:2c4421ed80358a0ee97b44357b6cd6dc09be6ccc27dfe9d50c9bfc39a760e5fe`
  - Pod name: `kubernetes-dashboard-5b575fd4c-77fqr`
  - Kubernetes Deployment UID: `d60f089f-0a85-11e8-9462-06d7638bd978`
  - Runtime (Docker) Container ID: `a3c7cd6d8e0ebf603b431604ab7a1844f8ed0b651b231b10849010fd7cf37b17`

Using just this information from cAdvisor, it's possible to gather a tremendous amount of information about the node's Host OS, the Network interface names, the CPU/RAM/Net/Disk utilization (over time), the processes running, and how long they've been running.  What a helpful service!

### Probe the "Secure" `Kubernetes API` service:

Verify the port is responding:
```
$ nc -vz $CONTROLLERIP 6443
Connection to 54.89.108.72 port 6443 [tcp/sun-sr-https] succeeded!
```

TODO

### Probe the "Insecure" `Kubernetes API` service:

Verify the port is responding:
```
$ nc -vz $CONTROLLERIP 8080
Connection to 54.89.108.72 port 8080 [tcp/http-alt] succeeded!
```

In this case, the "secure" port and "insecure" port are identical in functionality.

### Probe the `Kubelet Healthz` service:

Verify the port is responding:
```
$ nc -vz $CONTROLLERIP 10248
nc: connectx to 54.89.108.72 port 10248 (tcp) failed: Connection refused
```

It's only running on the `localhost` address:
```
$ ssh -i hkfs.pem ubuntu@$CONTROLLERIP
ubuntu@ip-10-1-0-10:~$ curl localhost:10248/
404 page not found
ubuntu@ip-10-1-0-10:~$ curl localhost:10248/healthz
ok
```

### Probe the `Kube-Proxy Metrics` service:

Verify the port is responding:
```
$ nc -vz $CONTROLLERIP 10249
nc: connectx to 54.89.108.72 port 10249 (tcp) failed: Connection refused
```

It's only running on the `localhost` address:
```
$ ssh -i hkfs.pem ubuntu@$CONTROLLERIP
ubuntu@ip-10-1-0-10:~$ curl localhost:10249/healthz
ok
ubuntu@ip-10-1-0-10:~$ curl localhost:10249/metrics
```

```ubuntu@ip-10-1-0-10:~$``` [```curl localhost:10249/metrics ```](cmd/kube-proxy-metrics.md)

It can tell us how `kube-proxy` reaches the API server on `10.1.0.10:8080` without encryption necessary.

### Probe the `Kubelet Read/Write` service:

Verify the port is responding:
```
$ nc -vz $CONTROLLERIP 10250
Connection to 54.89.108.72 port 10250 [tcp/*] succeeded!
```

Hit `tcp/10250` via curl:

```
$ curl $CONTROLLERIP:10250

$ curl -sk https://$CONTROLLERIP:10250
404 page not found
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



### Probe the `Kubernetes Scheduler HTTP` service:

Verify the port is responding:
```
$ nc -vz $CONTROLLERIP 10251
Connection to 54.89.108.72 port 10251 [tcp/*] succeeded!
```
Hit `tcp/10251` via curl:
```
$ curl $CONTROLLERIP:10251
404 page not found
$ curl $CONTROLLERIP:10251/healthz
ok
```

### Probe the `Kubernetes Controller Manager` service:

Verify the port is responding:
```
$ nc -vz $CONTROLLERIP 10252
Connection to 54.89.108.72 port 10252 [tcp/apollo-relay] succeeded!
```
Hit `tcp/10252` via curl:
```
$ curl $CONTROLLERIP:10252
404 page not found
$ curl $CONTROLLERIP:10252/healthz
ok
```

### Probe the `Kubelet Read-Only` service:

Verify the port is responding:
```
$ nc -vz $CONTROLLERIP 10255
Connection to 54.89.108.72 port 10255 [tcp/*] succeeded!
```
According to the source code, the following endpoints are available on the Kubelet "read-only" API:

- `/metrics`
- `/metrics/cadvisor`
- `/spec/`
- `/stats/`

Hit `tcp/10255` via curl:

```
$ curl $CONTROLLERIP:10255
404 page not found
```
The `/healthz` endpoint reports the health of the Kubelet. 
```
$ curl $CONTROLLERIP:10255/healthz
ok
```
The `/metrics` from the Kubelet indicate how busy the node is in terms of the `docker` runtime and how many containers "churn" on this node.

[```$ curl $CONTROLLERIP:10255/metrics ```](cmd/kubelet-metrics.md)

The `/metrics/cadvisor` endpoint passes through the metrics from the cAdvisor port.

[```$ curl $CONTROLLERIP:10255/metrics/cadvisor```](cmd/kubelet-metrics-cadvisor.md)

 The `/spec/` endpoint writes the cAdvisor `MachineInfo()` output, and this gives a couple hints that it runs on AWS, the instance type, and the instance ID:

[```$ curl $CONTROLLERIP:10255/spec/ ```](cmd/kubelet-spec.md)

The `/pods` endpoint provides the near-equivalent of `kubectl get pods -o json` for the pods running on this node:

[`$ curl -s $CONTROLLERIP:10255/pods`](cmd/kubelet-pods.md)

### Probe the `Kube-Proxy Healthcheck` service:

Verify the port is responding:
```
$ nc -vz $CONTROLLERIP 10256
Connection to 54.89.108.72 port 10256 [tcp/*] succeeded!
```
Hit `tcp/10256` via curl:
```
$ curl $CONTROLLERIP:10256
404 page not found
$ curl $CONTROLLERIP:10256/healthz
{"lastUpdated": "2018-03-27 21:17:09.14039841 +0000 UTC m=+888081.767665429","currentTime": "2018-03-27 21:17:36.679120214 +0000 UTC m=+888109.306387163"}
```
[Back](/README.md) | [Next](direct-worker.md)