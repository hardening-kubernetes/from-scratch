# Deploy the Azure Vote App

Let's also deploy a "realistic" application with multiple moving parts.  The Azure "Vote" Application has a web frontend with a simple Redis backend to store the vote tallies.  Installing it is simple:
```
$ cat > azure-vote.yml <<EOF
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  minReadySeconds: 5 
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 250m
          limits:
            cpu: 500m
        env:
        - name: REDIS
          value: "azure-vote-back"
EOF
```

And deploy the application resources:
```
$ kubectl apply -f azure-vote.yml 
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
```

Validate that the applications are running in the `default` namespace:
```
$ kubectl get pods
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-655476c7f7-ztdxk    1/1       Running   0          1m
azure-vote-front-764cff8457-t2ttl   1/1       Running   0          1m
nginx-7587c6fdb6-qjthq              1/1       Running   0          8m
```

View the "Vote" application in your local browser:
```
$ kubectl port-forward $(kubectl get pods -lapp=azure-vote-front -o jsonpath='{.items[0].metadata.name}') 8000:80
```
And then visit [http://localhost:8000](http://localhost:8000) in your browser to see the voting application and register a few votes for `cats` and `dogs`.  This way, the `redis` instance will have some "real data" in it.


[Back](/README.md#level-1-attacks)
