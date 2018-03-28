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

### Probe the `cAdvisor` port:

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
```
$ curl $CONTROLLERIP:4194/metrics
```
View the [Full Metrics Output](cmd/cadvisor-metrics.md)

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


### Probe the "Secure" `Kubernetes API` port:

Verify the port is responding:
```
$ nc -vz $CONTROLLERIP 6443
Connection to 54.89.108.72 port 6443 [tcp/sun-sr-https] succeeded!
```

TODO

### Probe the "Insecure" `Kubernetes API` port:

Verify the port is responding:
```
$ nc -vz $CONTROLLERIP 8080
Connection to 54.89.108.72 port 8080 [tcp/http-alt] succeeded!
```

In this case, the "secure" port and "insecure" port are identical in functionality.

### Probe the `Kubelet Healthz` port:

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

### Probe the `Kube-Proxy Metrics` port:

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
...snip...
rest_client_requests_total{code="200",host="10.1.0.10:8080",method="GET"} 3958
rest_client_requests_total{code="201",host="10.1.0.10:8080",method="POST"} 1
rest_client_requests_total{code="<error>",host="10.1.0.10:8080",method="GET"} 23
rest_client_requests_total{code="<error>",host="10.1.0.10:8080",method="POST"} 2
```

It can tell us how `kube-proxy` reaches the API server on `10.1.0.10:8080` without encryption necessary.

### Probe the `Kubelet Read/Write` port:

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

According to the source code, the following endpoints are available on the Kubelet "read-only" and "read/write" API:

- `/metrics`
- `/metrics/cadvisor`
- `/spec/`
- `/stats/`

The following endpoints are only available on the Kubelet's "read/write" API when the "debugging handlers" are enabled (which is the default):
- `/logs/` - Get logs from a pod/container.
- `/run/` - Alias for `/exec/`
- `/exec/` - Exec a command in a running container
- `/attach/` - Attach to the `stdout` of a running container
- `/portForward/` - Forward a port directly to a container
- `/containerLogs/` - Get logs from a pod/container.
- `/runningpods/` - Lists all running pods in short JSON form
- `/debug/pprof/` - Various go debugging performance endpoints



### Probe the `Kubernetes Scheduler HTTP` port:

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

### Probe the `Kubernetes Controller Manager` port:

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

### Probe the `Kubelet Read-Only` port:

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
```
$ curl $CONTROLLERIP:10255/metrics
...snip...
kubelet_docker_operations{operation_type="list_containers"} 3.27638e+06
kubelet_docker_operations{operation_type="list_images"} 124267
kubelet_docker_operations{operation_type="pull_image"} 92
kubelet_docker_operations{operation_type="remove_container"} 111
kubelet_docker_operations{operation_type="start_container"} 115
kubelet_docker_operations{operation_type="stop_container"} 155
...snip...
```
The `/metrics/cadvisor` endpoint passes through the metrics from the cAdvisor port.
```
$ curl $CONTROLLERIP:10255/metrics/cadvisor
...snip...
...snip...
```
 The `/spec/` endpoint writes the cAdvisor `MachineInfo()` output, and this gives a couple hints that it runs on AWS, the instance type, and the instance ID:
```
$ curl $CONTROLLERIP:10255/spec/
{
  "num_cores": 1,
  "cpu_frequency_khz": 2400088,
  "memory_capacity": 2095865856,
  "hugepages": [
   {
    "page_size": 2048,
    "num_pages": 0
   }
  ],
  "machine_id": "9388fcdf27304278a02f83a9c2d5333d",
  "system_uuid": "EC2826BA-6C3E-72DF-F19A-D389212A0C7D",
  "boot_id": "57596319-1c3e-4bb9-9514-97e9a9ccede6",
  "filesystems": [
   {
    "device": "/dev/xvda1",
    "capacity": 33240739840,
    "type": "vfs",
    "inodes": 4096000,
    "has_inodes": true
   },
   {
    "device": "tmpfs",
    "capacity": 209588224,
    "type": "vfs",
    "inodes": 255843,
    "has_inodes": true
   }
  ],
  "disk_map": {
   "202:0": {
    "name": "xvda",
    "major": 202,
    "minor": 0,
    "size": 34359738368,
    "scheduler": "none"
   }
  },
  "network_devices": [
   {
    "name": "eth0",
    "mac_address": "06:d7:63:8b:d9:78",
    "speed": 0,
    "mtu": 9001
   }
  ],
  "topology": [
   {
    "node_id": 0,
    "memory": 2095865856,
    "cores": [
     {
      "core_id": 0,
      "thread_ids": [
       0
      ],
      "caches": [
       {
        "size": 32768,
        "type": "Data",
        "level": 1
       },
       {
        "size": 32768,
        "type": "Instruction",
        "level": 1
       },
       {
        "size": 262144,
        "type": "Unified",
        "level": 2
       }
      ]
     }
    ],
    "caches": [
     {
      "size": 31457280,
      "type": "Unified",
      "level": 3
     }
    ]
   }
  ],
  "cloud_provider": "AWS",
  "instance_type": "t2.small",
  "instance_id": "i-05bb925bb00bf3bcc"
 }
```

The `/pods` endpoint provides the near-equivalent of `kubectl get pods -o json` for the pods running on this node:

[`curl -s $CONTROLLERIP:10255/pods`](cmd/kubelet-pods.md)



### Probe the `Kube-Proxy Healthcheck` port:

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