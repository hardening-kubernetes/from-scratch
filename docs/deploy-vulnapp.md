# Deploy a "Vulnerable" Application

While we could deploy a pod/container that has a remote code execution vulnerability to demonstrate the risks of a compromised container, we can save ourselves some time and effort by just deploying a standard web application pod and leveraging `kubectl exec` to gain a shell in that container as if we went to the trouble to exploit a web application.  The second benefit is that we don't actually have to expose this container, so this exercise is very controlled.

Let's deploy a very standard `nginx` container in the `default` namespace:
```
$ kubectl run nginx --image=nginx --replicas=1 --port=80
deployment "nginx" created
```

```
$ kubectl get pods
NAME                     READY     STATUS    RESTARTS   AGE
nginx-7587c6fdb6-qjthq   1/1       Running   0          35s
```

View the default nginx welcome message in your local browser:
```
$ kubectl port-forward $(kubectl get pods -lrun=nginx -o jsonpath='{.items[0].metadata.name}') 8000:80
```
And then visit [http://localhost:8000](http://localhost:8000) in your browser to see the default nginx application.

For now, we'll just leave this here.  But very soon this will become a dangerous launching point that emphasizes the importance of a defense-in-depth security approach in Kubernetes clusters of any importance.

[Back](/README.md#deploy-application-workloads) | [Next](deploy-voteapp.md)
