# Python Flask App + MongoDB on K8S.

Simple hello world python Flask application with MongoDB deployed on Kubernetes.
This tutorial is tried on Minikube (Local Kubernetes) on Mac OSX.

# Prerequesites

Please following the installation process from kubernetes official page:
https://kubernetes.io/docs/tasks/tools/install-minikube/

# Python Web App

Let's start by building up our Python Flask Docker image. Start up Docker Deamon and open a terminal and run the following command within the app folder:

```bash
docker build -f Dockerfile -t python-flask:latest .
```

Before deploying this app on K8S, let's try to run our application using the Docker CLI. For this, on the terminal again:

```bash
docker run -p 80:5000 python-flask
```

Let's try to access our Python Flask API using Terminal's curl command

```bash
curl http://localhost
```

# Push to Registry (Optional)

If you have a Docker Hub Registry, you can push the builded image:

```bash
docker push zabazop/python-flask:lastest
```

# Deployment 

Let's deploy our Python Flask API to Kubernetes. For this we will be using deployment.yaml file in Kubernetes folder. Move to the kubernetes directory and run the command:

```bash
kubectl apply -f deployment.yaml  
```

Let's check the deployment of our application:

```bash
kubectl get pods

m.goncu$ kubectl get pods 
NAME                            READY   STATUS    RESTARTS   AGE
python-flask-74b99578ff-24w6h   1/1     Running   0          63m
python-flask-74b99578ff-gqgzf   1/1     Running   0          63m
```

To get more information on the pod, you can run:

```bash
# kubectl describe pod $pod_name$
kubectl describe pod python-flask-74b99578ff-24w6h
```

# Exposing Application

In this tutorial we will be using LoadBalancer as type of service. We could also do this by using another service type like ClusterIP. Again in the kubernetes directory:

```bash
kubectl apply -f service.yaml  
```

On cloud providers that support LoadBalancer type, the external IP will be provisioned to access the service. You can check the external IP using the following command:

```bash
kubectl get services
```

You will have something like the following (with external IP of course):

```bash
m.goncu$ kubectl get service python-flask-service
NAME                   TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
python-flask-service   LoadBalancer   10.106.93.116   <pending>     6000:32405/TCP   29s
```

If you are using minikube, the usage of the LoadBalancer is a little bit different. The LoadBalancer service type is accessible through the minikube service command. To access it, run the following command:

```bash
minikube service python-flask-service
```

This will get you the following output:

```bash
m.goncu$ minikube service python-flask-service
|-----------|----------------------|-------------|-----------------------------|
| NAMESPACE |         NAME         | TARGET PORT |             URL             |
|-----------|----------------------|-------------|-----------------------------|
| default   | python-flask-service |             | http://192.168.99.100:31466 |
|-----------|----------------------|-------------|-----------------------------|
🎉  Opening service default/python-flask-service in default browser...
```

Kubernetes Documentation is great on the different service types, here's the appropriate page:
https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types

Here's great read for the different types of services in Kubernetes: https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0


# Clean Up

To delete the deployed application and the service we can use the following commands

```bash
# Delete the application
kubectl delete -n default deployment python-flask

# Delete the LoadBalancer Service 
kubectl delete service python-flask-service
```


# Useful Kubectl Command

See the logs of the running pod:

```bash
kubectl logs -f $pod_name$
```