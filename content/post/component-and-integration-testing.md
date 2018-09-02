+++
date = 2018-09-01
draft = false
tags = ["java", "spring boot", "docker", "docker-compose"]
title = "Component and Integration Testing with Docker"
summary = """Describes the approach for creating independent tests to check your service as a whole"""
+++

## Introduction
Nowadays more and more people use containers in their day to day job.
This technology shift doesn't have only impact on the way how we develop and deploy our services but the way how we test them.  
In this post I'd like to show you how we can benefit from `Docker` and `docker-compose` when creating component and integration tests.


The idea is to have our tests independent from the service source code, so once we do changes to the service source code our component tests should remain the same.  
To achieve this criteria we have to keep our tests as a standalone project in the same [git repository](https://github.com/isaranchuk/integration-testing-demo/tree/master/integration) and test our service as a blackbox by checking only its API.  
Because API is the thing you wouldn't like to break as you want to make your clients happy.

## Use case
For demo purposes I've created a simple microservice to generate activities that you can do when you're bored.  
The service exposes two REST endpoints:  

* `GET /v1/users/{username}/activities/any` to get any activity for the user when he's bored
* `GET /v1/users/{username}/activities/history` to list activities history for the user

Our service doesn't know how to generate activities, so for that we use a third party service [The Bored API](https://www.boredapi.com/) and we will use [Redis](https://redis.io/) to store activities history.  
Below you can see containers diagram for our microservice:

![activities containers diagram](/img/activities_service_container_diagram.png)

You can find Activities Service source code with all the instructions [here](https://github.com/isaranchuk/integration-testing-demo).

## Testing
In microservices architecture components are microservices themselves, so our component test will check our service as a whole.  
But the problem is that Activities Service depends on external systems (surprise!).  
And here `Docker` and in particular `docker-compose` come into the play by completely preparing the whole integration environment, you can check [docker-compose.yml](https://github.com/isaranchuk/integration-testing-demo/blob/master/integration/docker-compose.yml) for the details.  

The whole list of technologies that are used for testing:

* Docker and docker-compose
* Cucumber
* Wiremock
* RestAssured

## Summary
* With this kind of tests we can easily check the integration with third party services in controlled and reproducible way.
* The same `docker-compose` environment can be used as development or testing environment.
* Tests can be a shared responsibility for development and QA teams. Let's say developers are responsible for setting up test project with main test scenarios and later QA can pick it up and add more tests.
* It's easy to integrate component testing into CI/CD pipeline, you can check test script example [here](https://github.com/isaranchuk/integration-testing-demo/blob/master/bin/test-image.sh)
