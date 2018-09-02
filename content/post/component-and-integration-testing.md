---
date: 2018-07-01
title: "Component and Integration Testing"
date: 2018-09-02T09:41:20+03:00
draft: true
---

# Introduction
Nowadays more and more people use containers in their day to day job.
This technology shift doesn't have only impact on the way how we develop and deploy our services but the way how we test them.  
In this post I'd like to show how we can benefit from `Docker` and `docker-compose` when creating component and integration tests.

# Use case
For demo purposes I've created a simple microservice to generate activities that you can do when you're bored.  
The service exposes two REST endpoints:
* `GET /v1/users/{username}/activities/any` to get any activity for the user when he's bored
* `GET /v1/users/{username}/activities/history` to list activities history for the specified user
