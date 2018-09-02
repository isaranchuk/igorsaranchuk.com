+++
date = 2018-07-01
draft = false
tags = ["java", "spring boot", "kubernetes"]
title = "Deploy Spring Boot web service on Kubernetes"
summary = """A short demo how to deploy Spring Boot web service on Kubernetes."""
+++

## Introduction
Spring Boot is a breath of fresh air in enterprise Java world and Kubernetes is de facto a standard in container orchestration.
So in this post I'd like to show you my way of getting started with these technologies.

My app will be a simple Spring Boot web application with a single REST endpoint `/hello` that will return `Hello Spring Boot and Kubernetes`.
I agree not that much, but still it will show how to marry these two technologies.

If you are not familiar with Kubernetes basics then I would recommend you to follow the official tutorial:
https://kubernetes.io/docs/tutorials/kubernetes-basics/

Web application source code can be found on GitHub: https://github.com/isaranchuk/spring-boot-kubernetes

## Prerequisites
Before we get started you have to set up you local environment properly.

### Docker
This tutorial requires Docker to build and run images:
https://docs.docker.com/install/#supported-platforms

### Java and Maven
Our main goal is to run Java app on Kubernetes, so you need Java (here we use Java 8) and of course Maven to build our app:

#### Java
Follow the link below to install download Java:
https://java.com/en/download/

#### Maven
Check Maven installation instructions:
https://maven.apache.org/install.html

### kubectl
Install Kubernetes command-line tool:  
https://kubernetes.io/docs/tasks/tools/install-kubectl/

### Minikube
Install a single-node Kubernetes cluster to run Kubernetes locally:  
https://kubernetes.io/docs/tasks/tools/install-minikube/

#### Minikube tips
To be able to work with the docker daemon on your mac/linux host use the `docker-env` command in your shell:
```bash
eval $(minikube docker-env)
```
Now you should see Docker images on Minikube:
```bash
docker ps
```

NOTE: You have to run eval `$(minikube docker-env)` on each terminal you want to use, since it only sets the environment variables for the current shell session.

### Getting started
Once our local environment is up and running we can proceed with running our "Hello World" app.  
The first thing we need to do is to build docker image that will contain our web service.

```bash
./bin/build-image.sh
```

The two key Kubernetes concepts that we're going to use here are Deployment and Service, you can examine `app.yaml` to check their declaration.  

The Deployment instructs Kubernetes how to create and update instances of your application. Once you've created a Deployment, the Kubernetes master schedules mentioned application instances onto individual Nodes in the cluster.

A Service in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them. Services enable a loose coupling between dependent Pods.

Run the following command to create Deployment and Service for our application:
```bash
kubectl create -f app.yaml
```

#### Check Deployment
List of all the deployed objects can be obtained via:

```bash
kubectl get deployment
```

Check details of `webapp1` deployment:

```bash
kubectl describe deployment webapp1
```

#### Check Service
List of all service objects can be obtained via:

```bash
kubectl get svc
```

Check details of `webapp1-svc` service:
```bash
kubectl describe svc webapp1-svc
```

#### Call REST endpoint
First of all we have to know Minikube IP, so we can refer to our application:

```bash
minikube ip
```

Then we can call our REST endpoint with any http client we like:
```bash
curl {minikube_ip}:32555/hello
```
If everything is good you will see `Hello Spring Boot and Kubernetes` message otherwise something is wrong.  
In case of any issues check troubleshooting tips below.

#### Troubleshooting tips
Once you created Deployment Kubernetes will create pod:
```bash
kubectl get pod
```
It is very useful to get pod details to check its status and related events:
```bash
kubectl describe pod {pod_name}
```

Also I found useful to check the actual logs of my application:
```bash
kubectl logs {pod_name}
```
