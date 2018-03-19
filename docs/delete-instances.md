# Delete Instances

## ```worker-1``` and ```worker-2``` Deletion

Set some ENV variables
```
$ export AWS_DEFAULT_REGION="us-east-1"
$ export KEY_NAME="hkfs"
```

Obtain the Worker Instance IDs
```
$ INSTANCE1_ID="$(aws ec2 describe-instances 
  --region ${AWS_DEFAULT_REGION} 
  --filter 'Name=tag:Name,Values=worker-1' 
  --query 'Reservations[].Instances[].InstanceId' 
  --output text)"
$ INSTANCE2_ID="$(aws ec2 describe-instances 
  --region ${AWS_DEFAULT_REGION} 
  --filter 'Name=tag:Name,Values=worker-2' 
  --query 'Reservations[].Instances[].InstanceId' 
  --output text)"
```

Terminate ```worker-1``` and ```worker-2```
```
$ aws ec2 terminate-instances \
  --region ${AWS_DEFAULT_REGION} \
  --instance-ids ${INSTANCE1_ID}
$ aws ec2 terminate-instances \
  --region ${AWS_DEFAULT_REGION} 
  --instance-ids ${INSTANCE2_ID}
```

## ```controller``` Deletion

Obtain the ```controller``` Instance ID and Terminate
```
$ INSTANCEM_ID="$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=controller' \
  --query 'Reservations[].Instances[].InstanceId' \
  --output text)"
$ aws ec2 terminate-instances \
  --region ${AWS_DEFAULT_REGION} \
  --instance-ids ${INSTANCEM_ID}
```

## ```etcd``` Deletion

Obtain the ```etcd``` Instance ID and Terminate

```
$ INSTANCEE_ID="$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=etcd' \
  --query 'Reservations[].Instances[].InstanceId' \
  --output text)"
$ aws ec2 terminate-instances \
  --region ${AWS_DEFAULT_REGION} \
  --instance-ids ${INSTANCEE_ID}
```

SSH Keypair Deletion
```
$ aws ec2 delete-key-pair --region ${AWS_DEFAULT_REGION} --key-name "${KEY_NAME}"
```

[Back](/README.md) | [Next](delete-vpc.md)
