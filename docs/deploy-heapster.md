# Deploy Heapster

From the same installation system shell, create the ```kube-dns.yml``` Definition

```
$ cat > heapster.yml <<EOF
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
        - --source=kubernetes:http://10.1.0.10:8080?inClusterConfig=false&useServiceAccount=false
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
$ kubectl create -f heapster.yml
```

## Validation
```
$ kubectl get pods --all-namespaces
NAMESPACE     NAME                        READY     STATUS    RESTARTS   AGE
kube-system   heapster-d7c4b86d4-mdsps    1/1       Running   0          1m
kube-system   kube-dns-68886d5985-sht75   3/3       Running   0          2m
```

[Back](/README.md#level-0-security) | [Next](deploy-basic-dashboard.md)
