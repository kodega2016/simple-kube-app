So, we can run the kubernetes pods using imperative and declarative approach. First of all, lets explore how we can use declarative approach to run the pods.

Lets run a simple nginx web server in a pod using declarative approach.

```bash
kubectl run webserver --image nginx --port 80
```

now,lets check the available pods in out cluster.

```bash
kubectl get pods
```

it will logs all the available pods in the cluster.

We can view the details of the pod using the following command.

```bash
kubectl describe pod webserver
```

Here, the pod is not accessible from outside the cluster, to make it accessible we need to expose it as a service.

```bash
kubectl expose pods webserver --type LoadBalance --port 80 --target-port 80
```

it will expose the pod as a service and make it accessible from outside the cluster.so we can access the pod using the external IP.

```bash
minikube service webserver
```

it will open the webserver in the default browser.
