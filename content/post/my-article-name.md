+++
date = 2016-04-10
draft = false
tags = ["java", "spring boot", "kubernetes"]
title = "Deploy Spring Boot web service on Kubernetes"
summary = """A short demo how to deploy Spring Boot web service on Kubernetes."""
+++

# Introduction
Spring Boot is a fresh breath in enterpraise Java world and Kubernetest is de facto a standarad in container orchestration.
So in this post I'd like to show you my way to get started with these technologies.

Our app will be a simple Spring Boot web application with a single REST endpoint `/hello` that will return `Hello Spring Boot and Kubernetes`.
I agree not that much, but still it will show how we can merry this two technologies.

If you are not familiar with Kubernetes basics then I would recommend to follow the official tutorial:
https://kubernetes.io/docs/tutorials/kubernetes-basics/

# Prerequisites
Before we get started you have to set up you local environment to run this tutorial.

## Docker
This tutorial requires Docker to build and run images:
https://docs.docker.com/install/#supported-platforms

## Java and Maven
Our main goal is to run java app on Kubernetes so you need Java (here we use Java 8) and of course Maven to build our app:

### Java
https://java.com/en/download/

### Maven
https://maven.apache.org/install.html

## kubectl
Install Kubernetes command-line tool:  
https://kubernetes.io/docs/tasks/tools/install-kubectl/

## Minikube
Install a single-node Kubernetes cluster to run Kubernetes locally:  
https://kubernetes.io/docs/tasks/tools/install-minikube/

### Minikube tips
To be able to work with the docker daemon on your mac/linux host use the docker-env command in your shell:
```bash
eval $(minikube docker-env)
```
Now you should see Docker images on Minikube:
```bash
docker ps
```

NOTE: You have to run eval $(minikube docker-env) on each terminal you want to use, since it only sets the environment variables for the current shell session.

## Getting started
Once our local environment is up and running we can proceed with running our "Hello World" app.
The first thing we have to do is to build docker images that will contain our web service.

```bash
./bin/build-image.sh
```
