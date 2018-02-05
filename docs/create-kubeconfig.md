# Create the local ```kubeconfig``` file


From the same installation system shell, obtain the master system IP
```
MASTER_IP=$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=master' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

Create the ```kubeconfig``` file locally
```
cat > kubeconfig <<EOF
apiVersion: v1
clusters:
- cluster:
    server: http://${MASTER_IP}:8080
  name: default
contexts:
- context:
    cluster: default
    user: system:node:master
  name: default
current-context: "default"
kind: Config
preferences: {}
users:
- name: system:node:master
  user:
    as-user-extra: {}
EOF
```

export the location of the ```KUBECONFIG```
```
export KUBECONFIG=./kubeconfig
```

Verify remote API access
```
kubectl get nodes
NAME           STATUS    ROLES     AGE      VERSION
ip-10-1-0-10   Ready     <none>    9m       v1.9.2
ip-10-1-0-11   Ready     <none>    6m       v1.9.2
ip-10-1-0-12   Ready     <none>    1m       v1.9.2
```

[Back](/README.md) | [Next](deploy-kube-dns.md)
