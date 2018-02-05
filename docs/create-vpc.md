# Launch and Configure the VPC

## Cloudformation Template

From inside the repo directory on your installation system, set a few environment variables.  Note: You will need the AMI for your region to be correct.  See: [Ubuntu 16.0.4 LTS](http://cloud-images.ubuntu.com/locator/ec2/) and search for '16.04 LTS hvm:ebs-ssd'
```
export STACK_NAME="hkfs"
export AWS_DEFAULT_REGION="us-east-1"
export KEY_NAME="hkfs"
export IMAGE_ID="ami-66506c1c"
```

Deploy the Cloudformation template to create the VPC, subnet, route table, route table association, and internet gateway.
```
aws cloudformation create-stack --region ${AWS_DEFAULT_REGION} --stack-name ${STACK_NAME} \
  --template-body file://templates/${STACK_NAME}.json --output text
```

Obtain the VPC Id
```
VPC_ID="$(aws cloudformation describe-stacks --region ${AWS_DEFAULT_REGION} \
  --query 'Stacks[*].Outputs[?OutputKey==`VPCId`].OutputValue[]' \
  --stack-name ${STACK_NAME} --output text)"
echo "${VPC_ID}"
```

Obtain the Security Group Id
```
SG_ID="$(aws ec2 describe-security-groups --query 'SecurityGroups[*].GroupId' \
  --region ${AWS_DEFAULT_REGION} --filter "Name=vpc-id,Values=${VPC_ID}" --output text)"
echo "${SG_ID}"
```

Obtain the Subnet Id
```
SUBNET_ID="$(aws ec2 describe-subnets --region ${AWS_DEFAULT_REGION} \
  --filter "Name=tag:Name,Values=${STACK_NAME}-subnet" --query 'Subnets[*].SubnetId' \
  --output text)"
echo "${SUBNET_ID}"
```

Allow just one IP to access the cluster instances.
```
aws ec2 authorize-security-group-ingress --region ${AWS_DEFAULT_REGION} --group-id ${SG_ID} \
  --protocol all --port 0 --cidr $(dig +short myip.opendns.com @resolver1.opendns.com)/32
```

Create a new SSH keypair
```
aws ec2 create-key-pair --region ${AWS_DEFAULT_REGION} --key-name ${KEY_NAME} \
  --query 'KeyMaterial' --output text > ${KEY_NAME}.pem
chmod 600 ${KEY_NAME}.pem
```

[Back](/README.md) | [Next](launch-configure-etcd.md)
