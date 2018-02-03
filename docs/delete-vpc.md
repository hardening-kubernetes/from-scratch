# Delete VPC

Set some ENV variables
```
export AWS_DEFAULT_REGION="us-east-1"
export STACK_NAME="hkfs"
```

Destroy the VPC via Cloudformation stack deletion
```
aws cloudformation delete-stack --region ${AWS_DEFAULT_REGION} --stack-name ${STACK_NAME}
```

[Back](README.md)
