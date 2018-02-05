# Deploy Heapster

SSH Into the ```master``` Instance
```
ssh -i ${KEY_NAME}.pem ubuntu@$(aws ec2 describe-instances \
  --region ${AWS_DEFAULT_REGION} \
  --filter 'Name=tag:Name,Values=master' \
  --query 'Reservations[].Instances[].NetworkInterfaces[0].Association.PublicIp' \
  --output text)
```

Create the ```kube-dns.yml``` Definition

```
cat > heapster.yml <<EOF
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: heapster
  namespace: kube-system
spec:
  replicas: 1
  template:
    metadata:
      labels:
        task: monitoring
        k8s-app: heapster
    spec:
      serviceAccountName: heapster
      containers:
      - name: heapster
        image: k8s.gcr.io/heapster-amd64:v1.4.2
        imagePullPolicy: IfNotPresent
        command:
        - /heapster
        - --source=kubernetes:http://kubernetes.default?inClusterConfig=false&useServiceAccount=false
---
apiVersion: v1
kind: Service
metadata:
  labels:
    task: monitoring
    kubernetes.io/cluster-service: 'true'
    kubernetes.io/name: Heapster
  name: heapster
  namespace: kube-system
spec:
  ports:
  - port: 80
    targetPort: 8082
  selector:
    k8s-app: heapster
EOF
```

## Deployment
```
kubectl create -f heapster.yml
```

## Validation
```
kubectl get pods --all-namespaces
NAMESPACE     NAME                        READY     STATUS    RESTARTS   AGE
```

[Back](/README.md) | [Next](deploy-basic-dashboard.md)
