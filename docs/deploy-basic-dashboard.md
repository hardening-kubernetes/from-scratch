# Deploy the Kubernetes Dashboard

From the same installation system shell, create the ```basic-dashboard.yml``` Definition

```
cat > basic-dashboard.yml <<EOF
kind: Deployment
apiVersion: extensions/v1beta1
metadata:
  labels:
    k8s-addon: kubernetes-dashboard.addons.k8s.io
    k8s-app: kubernetes-dashboard
    version: v1.6.3
    kubernetes.io/cluster-service: "true"
  name: kubernetes-dashboard
  namespace: kube-system
spec:
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      k8s-app: kubernetes-dashboard
  template:
    metadata:
      labels:
        k8s-addon: kubernetes-dashboard.addons.k8s.io
        k8s-app: kubernetes-dashboard
        version: v1.6.3
        kubernetes.io/cluster-service: "true"
      annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ''
        scheduler.alpha.kubernetes.io/tolerations: '[{"key":"CriticalAddonsOnly", "operator":"Exists"}]'
    spec:
      containers:
      - name: kubernetes-dashboard
        image: gcr.io/google_containers/kubernetes-dashboard-amd64:v1.6.3
        ports:
        - containerPort: 9090
          protocol: TCP
        resources:
          # keep request = limit to keep this container in guaranteed class
          limits:
            cpu: 100m
            memory: 50Mi
          requests:
            cpu: 100m
            memory: 50Mi
        args:
           - --apiserver-host=http://10.1.0.10:8080
        livenessProbe:
          httpGet:
            path: /
            port: 9090
          initialDelaySeconds: 30
          timeoutSeconds: 30
      serviceAccountName: kubernetes-dashboard
      tolerations:
      - key: node-role.kubernetes.io/master
        effect: NoSchedule
---
kind: Service
apiVersion: v1
metadata:
  labels:
    k8s-addon: kubernetes-dashboard.addons.k8s.io
    k8s-app: kubernetes-dashboard
    kubernetes.io/cluster-service: "true"
  name: kubernetes-dashboard
  namespace: kube-system
spec:
  ports:
  - port: 80
    targetPort: 9090
  selector:
    k8s-app: kubernetes-dashboard
EOF
```

## Deployment
```
kubectl create -f basic-dashboard.yml
```

## Validation
```
kubectl get pods --all-namespaces
NAMESPACE     NAME                                   READY     STATUS    RESTARTS   AGE
kube-system   heapster-555f7f75b6-vcz4p              1/1       Running   0          5m
kube-system   kube-dns-68886d5985-sht75              3/3       Running   0          7m
kube-system   kubernetes-dashboard-5b575fd4c-77fqr   1/1       Running   0          1m
```

Get the dashboard pod name
```
POD_NAME=$(kubectl get pods -l k8s-app=kubernetes-dashboard -n kube-system -o jsonpath="{.items[0].metadata.name}")
```

Open a local port (8000) forwarded to the pod port (9090)
```
kubectl port-forward -n kube-system $POD_NAME 8000:9090
Forwarding from 127.0.0.1:8000 -> 9090
```

In a browser, visit:
```
http://localhost:8000
```

[Back](/README.md) | [Next](deploy-.md)
