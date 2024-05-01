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

Now, lets try to create our first deployment with our own image.so first of all let's dockerize the application and push the docker image to docker registry.

```bash
docker image build . -t kodega2016/kube_action
docker image tag kodega2016/kube_action kodega2016/kube_action:latest
docker push kodega2016/kube_action:latest
```

So after pushing the image to the docker registry, we can create the deployment using the following command.

```bash
kubectl create deployment webapp_deployment --image kodega2016/kube_action:latest --port 8080
```

It will create the deployment with the specified image and port.We can check with the following commands.

```bash
kubectl get deployments
```

We can view the details of the deployments with the following command.

```bash
kubectl describe deployment webapp_deployment
```

Now,lets expose the deployment as a service to make it accessible from outside the cluster.

```bash
kubectl expose deployment webapp_deployment --type LoadBalancer --port 80 --target-port 80
```

Now, we can use the following minikube command to access the service from outside the cluster.

```bash
minikube service webapp_deployment
```

Scaling the deployment:
We can scale the deployment using the following command.

```bash
kubectl scale deployment webapp_deployment --replicas=3
```

It will scale the deployment to 3 replicas means 3 pods will be created with the same image and port.

Updating the dployment:
To update the deployment, we have to run the following command.

```bash
kubectl set image deployment/webapp-deployment kube-action=kodega2016/kube_action:v2
```

So, we can view the rollout status with the following commands.

```bash
kubectl rollout status deployment/webapp-deployment
```

we can view the rollout history of the deployment with the following commands:

```bash
kubectl rollout history deployment/webapp-deployment
```

we can view the rollout history details with the following command.

```bash
kubectl rollout history deployment webapp-deployment --revision=2
```

it will show the details of the specified revision and we can rollback to the previous deployment with the following command.

```bash
kubectl rollout undo deployment webapp-deployment --to-revision=1
```

it will rollback the deployment to the previous version(1)

So, another approach to define the confifuration of the deployment is using the yaml file, Here we have defined some yaml files inside `k8s` folder.
We need to following commands to create the deployment using the yaml file.

```bash
kubectl apply -f k8s/webapp-pod.yaml
```

it will create the pod with the specified configuration in the yaml file.
We can delete the pod using the following command.

```bash
kubectl delete -f k8s/webapp-pod.yaml
```

We can update the configuration in the yaml file and apply changes with the apply commands as mentioned above.
we can delete the resource using the label also,

```bash
kubectl delete deployments -l app=webapp
```

it will delete the deployment with the label app=webapp.
