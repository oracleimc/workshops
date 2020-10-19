# Container Engine for Kubernetes (OKE) on OCI Workshop #

## Introduction

Oracle Cloud Infrastructure Container Engine for Kubernetes is a fully-managed, scalable, and highly available service that you can use to deploy your containerized applications to the cloud. Use Container Engine for Kubernetes (sometimes abbreviated to just OKE) when your development team wants to reliably build, deploy, and manage cloud-native applications. 
 
Container Engine for Kubernetes uses Kubernetes - the open-source system for automating deployment, scaling, and management of containerized applications across clusters of hosts. Container Engine for Kubernetes is integrated with Oracle Cloud Infrastructure Identity and Access Management (IAM), which provides easy authentication with native Oracle Cloud Infrastructure identity functionality.

In this workshop, you will learn how to setup: 

+ **Set up Kubernetes , Policies**
+ **Deployments - Storage, service**
+ **OCI Customization** running on Springboot (Java)
+ **Rolling out updates** 

## Prerequisites ##

For this workshop you require and oracle cloud Account. You may request a trial account [here](https://myservices.us.oraclecloud.com/mycloud/signup?language=en&sourceType=_ref_coc-asset-opcHome) or if you are in an event an account maybe provided.

You will also need a **Github** account.

For software on your laptop you will require:

+ Github client - you can download [here](https://git-scm.com/downloads).
+ Visual Code (or IDE of your choice) - you can download [here](https://code.visualstudio.com/)

GitHub imports

+ import https://github.com/allenkubai/people-service.git into your github account. Use the same name
+ import https://github.com/allenkubai/people-web-app.git into your github account. Use the same name

## Steps to follow ##

1. Creating a Kubernetes Cluster on Oracle Cloud Infrastructure
    - Please note that if are using a instructor provided environment you might already have a cluster and you need to skip this step
    - If you want to create your own cluster you can create using the Quick create option. [Here](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/index.html) are the instructions
2. [Deployments - Storage](deployments.storage.OKE2.md)
3. [Create Jenkins Instance in your kubernetes cluster](jenkins.pipelines.OKE3.md)
4. [Configure the Jenkins instance](jenkins.pipelines.OKE4.md).
5. [Create Mysql Server in the kubernetes cluster](jenkins.pipelines.OKE5.md)
6. [Create a Shared Ingress for the kubernetes cluster](jenkins.pipelines.OKE6.md)
7. [Deploy the people-service backend API using Jenkins Pipelines](jenkins.pipelines.OKE7.md)
7. [Deploy the people-web-app frontend using Jenkins Pipelines](jenkins.pipelines.OKE8.md)
