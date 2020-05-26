# Python Flask App + MongoDB on K8S.

Simple hello world python Flask application with MongoDB deployed on Kubernetes.
This tutorial is tried on Minikube (Local Kubernetes) on Mac OSX.

# Installing Minikube on local machine

Please following the installation process from kubernetes official page:
https://kubernetes.io/docs/tasks/tools/install-minikube/

# Python Web App

Let's start by building up our Python Flask Docker image. Start up Docker Deamon and open a terminal and run the following command within the app folder:

```bash
docker build -f ../docker/Dockerfile -t python-flask:latest .
```

Before deploying this app on K8S, let's try to run our application using the Docker CLI. For this, on the terminal again:

```bash
docker run -p 80:5000 python-flask
```

Let's try to access our Python Flask API using Terminal's curl command

```bash
curl http://localhost
```

# Web App Deployment 

Let's deploy our Python Flask API to Kubernetes. For this we will be using deployment.yaml file in Kubernetes folder.

# Web App Service 

In this tutorial we will be using LoadBalancer as type of service. We could also do this using the ClusterIP service type.

Kubernetes Documentation is great on this, here's the appropriate page:
https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types

Here's great read for different types of services in Kubernetes: https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0

