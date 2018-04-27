# Level 1 Hardening - Security Groups

## Limit the services available externally

In the [creation of the VPC](create-vpc.md) instructions, the security group protecting the EC2 instances had this rule applied to simulate leaving everything "wide open" to the Internet:

```
$ aws ec2 authorize-security-group-ingress --region ${AWS_DEFAULT_REGION} \
  --group-id ${SG_ID} --protocol all --port 0 \
  --cidr $(dig +short myip.opendns.com @resolver1.opendns.com)/32
```

If we had used `0.0.0.0/0` , that would be true.  Instead we used our source IP in its place to not be completely irresponsible.

Given that direct access to the services running on the cluster in their current configuration is a `root`-privileged remote command execution vulnerability, it's time to change that.

So, let's do the least amount of work possible to remedy this.

Export the necessary variables:
```
$ export STACK_NAME="hkfs"
$ export AWS_DEFAULT_REGION="us-east-1"
$ export KEY_NAME="hkfs"
```
Obtain the Security Group ID:
```
$ VPC_ID="$(aws cloudformation describe-stacks --region ${AWS_DEFAULT_REGION} \
  --query 'Stacks[*].Outputs[?OutputKey==`VPCId`].OutputValue[]' \
  --stack-name ${STACK_NAME} --output text)"
$ SG_ID="$(aws ec2 describe-security-groups --query 'SecurityGroups[*].GroupId' \
  --region ${AWS_DEFAULT_REGION} --filter "Name=vpc-id,Values=${VPC_ID}" --output text)"
```

Add two rules which only allow `tcp/22` for SSH and `tcp/6443` from your source IP address instead of "all":
```  
$ aws ec2 authorize-security-group-ingress --region ${AWS_DEFAULT_REGION} \
 --group-id ${SG_ID} --protocol tcp --port 22 \
 --cidr $(dig +short myip.opendns.com @resolver1.opendns.com)/32
$ aws ec2 authorize-security-group-ingress --region ${AWS_DEFAULT_REGION} \
 --group-id ${SG_ID} --protocol tcp --port 6443 \
 --cidr $(dig +short myip.opendns.com @resolver1.opendns.com)/32
```
Remove the old rule that allowed "all" from your source IP:
```
$ aws ec2 revoke-security-group-ingress --region ${AWS_DEFAULT_REGION} \
 --group-id "${SG_ID}" --protocol all --port 0 \
 --cidr $(dig +short myip.opendns.com @resolver1.opendns.com)/32
```

Confirm you can still SSH into your nodes:
```
$ export ETCD_IP=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=etcd' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
$ export CONTROLLER_IP=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=controller' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
$ export WORKER1_IP=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=worker-1' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
$ export WORKER2_IP=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=worker-2' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
  
$ ssh -i ${KEY_NAME}.pem ubuntu@${ETCD_IP}
$ ssh -i ${KEY_NAME}.pem ubuntu@${CONTROLLER_IP}
$ ssh -i ${KEY_NAME}.pem ubuntu@${WORKER1_IP}
$ ssh -i ${KEY_NAME}.pem ubuntu@${WORKER2_IP}
```

Confirm that you can no longer reach `etcd`:
```
$ nc -vz ${ETCD_IP} 2379
nc: connectx to ETCD_IP port 2379 (tcp) failed: Operation timed out
```

Confirm that you can no longer reach the Insecure API Server:
```
$ nc -vz ${CONTROLLER_IP} 8080
nc: connectx to CONTROLLER_IP port 8080 (tcp) failed: Operation timed out
```

Confirm that you can no longer reach the Kubelet's Read/Write API Service:
```
$ nc -vz ${CONTROLLER_IP} 10250
nc: connectx to CONTROLLER_IP port 10250 (tcp) failed: Operation timed out
$ nc -vz ${WORKER1_IP} 10250
nc: connectx to WORKER1_IP port 10250 (tcp) failed: Operation timed out
$ nc -vz ${WORKER2_IP} 10250
nc: connectx to WORKER2_IP port 10250 (tcp) failed: Operation timed out
```

Finally, confirm that you can reach the API Server service on the secure port if it were running.  We should expect a "connection refused" error as the request reached the instance and sent a TCP reset because nothing is currently listening on `tcp/6443`:

```
$ nc -vz ${CONTROLLER_IP} 6443
nc: connectx to CONTROLLER_IP port 6443 (tcp) failed: Connection refused
```

Remember that the instance security group has itself listed as being able to access itself.  This means that since all instances have this same security group assigned, they can access each other directly.  The rules modified here apply to all "external" access control, and we've limited access to just `tcp/22` and `tcp/6443` from our source IP.

Now, it's time to generate and enable TLS on the Kubernetes API Server service listening on `tcp/6443`.

[Back](/README.md#level-1-hardening) | [Next](l1-api-tls.md)
