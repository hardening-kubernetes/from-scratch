# Access the Etcd Services Directly

## Services Available

- `22/tcp` - [SSH](https://openssh.org)
- `2379/tcp` - [Etcd server service](https://github.com/coreos/etcd#etcd-tcp-ports)
- `2380/tcp` - [Etcd peer discovery service](https://github.com/coreos/etcd#etcd-tcp-ports)

Set the `etcd` IP to a variable:
```
$ export ETCDIP=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=etcd' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

### Probe the `ssh` port:

Verify the port is responding:
```
$ nc -v $ETCDIP 22
Connection to 18.233.67.127 port 22 [tcp/ssh] succeeded!
SSH-2.0-OpenSSH_7.2p2 Ubuntu-4ubuntu2.4
```

### Probe the `etcd` server port:

Verify the port is responding:
```
$ nc -z $ETCDIP 2379
Connection to 18.233.67.127 port 2379 [tcp/*] succeeded!
```
From this, we know that an external IP can access the `etcd` server directly.  This shouldn't ever be the case.

Verify that the port is a web port:
```
$ curl $ETCDIP:2379
404 page not found
```
This tells us that `etcd` is not listening with TLS and it's reachable. This is catastrophically bad for the integrity of this cluster.

Obtain the `etcd` version:
```
$ curl $ETCDIP:2379/version
{"etcdserver":"3.2.11","etcdcluster":"3.2.0"}
$ ETCDCTL_API=3 ./etcdctl --endpoints "http://$ETCDIP:2379" version
etcdctl version: 3.3.2
API version: 3.3
```

Use `etcdctl` to get the `etcd` cluster health:
```
$ ETCDCTL_API=3 ./etcdctl --endpoints "http://$ETCDIP:2379" endpoint health
http://18.233.67.127:2379 is healthy: successfully committed proposal: took = 6.903553ms
```

Get the `etcd` cluster members:
```
$ ETCDCTL_API=3 ./etcdctl --endpoints "http://$ETCDIP:2379" member list
e0187950b759a617, started, ip-10-1-0-5, http://10.1.0.5:2380, http://10.1.0.5:2379
```

Dump the entire contents of `etcd`'s datastore:
```
$ ETCDCTL_API=3 ./etcdctl --endpoints "http://$ETCDIP:2379" get "" --from-key > etcd.raw
or
$ ETCDCTL_API=3 ./etcdctl --endpoints "http://$ETCDIP:2379" snapshot save etcd.dump
Snapshot saved at etcd.dump
```

Dig into the `etcd.dump` using `vi` or [boltbrowser](https://github.com/br0xen/boltbrowser):
```
$ vi etcd.raw
or
$ vi etcd.dump
or
$ ./boltbrowser etcd.dump
```
You now have a complete, bit-for-bit copy of the entire state of the cluster.  This includes pods, configmaps, secrets, and more.

### Probe the `etcd` peer discovery port:

Verify the port is responding:
```
$ nc -z $ETCDIP 2380
Connection to 18.233.67.127 port 2380 [tcp/*] succeeded!
```

Verify that the port is a web port:
```
$ curl $ETCDIP:2380
404 page not found
```

Obtain the `etcd` version:
```
$ curl $ETCDIP:2380/version
{"etcdserver":"3.2.11","etcdcluster":"3.2.0"}
$ ETCDCTL_API=3 ./etcdctl --endpoints "http://$ETCDIP:2379" version
etcdctl version: 3.3.2
API version: 3.3
```
The `etcd` discovery service should not be exposed to any systems but the `etcd` servers.

[Back](/README.md) | [Next](direct-controller.md)
