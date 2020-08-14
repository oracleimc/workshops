# DevOps: Deploying Microservices to OKE with Jenkins Pipelines (CI/CD) #

## Introduction

In this workshop, we will focus on a popular CI/CD tool called Jenkins and how it can be used for continuous delivery on OKE. Jenkins is an extensible automation server that can be used as a simple CI server or turned into the continuous delivery hub for any project. 

This labs are design for people with or with no experience on cloud native technologies like Jenkins, Docker and Kubernetes. In this workshops you will work with thess tools plus frameworks like ReatJS and Springboot.

In this workshop, you will deploy four microservices on to OKE: 

+ **Jenkins Server**
+ **Frontend app** running on ReactJS
+ **Backend app (API)** running on Springboot (Java)
+ **Mysql Server** 

## Prerequisites ##

For this workshop you require and Oracle cloud Account. You may request a trial account [here](https://myservices.us.oraclecloud.com/mycloud/signup?language=en&sourceType=_ref_coc-asset-opcHome) or if you are in an event an account maybe provided.

Please review [prerequisites](./prerequisites.md) in detail

### GitHub imports

+ Fork https://github.com/allenkubai/people-service.git into your github account. Use the same name
+ Fork https://github.com/allenkubai/people-web-app.git into your github account. Use the same name

Fork button is on top left on each repository. _You do not require to fork this repository_
![](./images/github-fork1.png)

## Workshop execution
- If this workshop to be executed on emeaccoe tenancy, please follow the steps in the [emeaccoe guide](./emeaccoe.md)
- If you are to execute the flows on region other than Frankfurt, please review steps in [region guide](./region.md)

## Steps to follow ##

1. Creating a Kubernetes Cluster on Oracle Cloud Infrastructure
    - Please note that if are using a instructor provided environment you might already have a cluster and you need to skip this step
    - If you want to [create your own cluster](jenkins.pipelines.OKE1.md)
2. [Initial Setup](jenkins.pipelines.OKE2.md) - *please note the kubernetes cluster need to be up and running for this step*.
3. [Create Jenkins Instance in your kubernetes cluster](jenkins.pipelines.OKE3.md)
4. [Configure the Jenkins instance](jenkins.pipelines.OKE4.md).
5. [Create Mysql Server in the kubernetes cluster](jenkins.pipelines.OKE5.md)
6. [Create a Shared Ingress for the kubernetes cluster](jenkins.pipelines.OKE6.md)
7. [Deploy the people-service backend API using Jenkins Pipelines](jenkins.pipelines.OKE7.md)
7. [Deploy the people-web-app frontend using Jenkins Pipelines](jenkins.pipelines.OKE8.md)
