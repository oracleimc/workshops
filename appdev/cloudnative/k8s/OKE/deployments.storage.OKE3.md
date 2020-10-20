# Container Engine for Kubernetes (OKE) on OCI Workshop #

## Introduction - Deployment: Persistent Storage - MySQL
In some cases, you might wanted data to live past the lifetime of a container. In this case, you would need persistent storage. A persistent volume offers persistent storage that enables your data to remain intact, regardless of whether the containers to which the storage is connected are terminated.

In this lab, we going to use a practical example, where you have a mysql database running on OKE and the data stored by the database should live past the lifetime of it's container. 

# Prerequisites
Make sure you have completed the [Initail Setup](initial.setup.OKE2.md).

# Create Mysql Server in the kubernetes cluster #
In this lab, you are going to set up a mysql server instance in your kubernetes cluster. For this lab, we are going to use the public docker image for mysql. Here are the summary of steps

+ Create a **Persistance Volume** for mysql
+ Create the mysql instance
+ Create the database in mysql

1. The first thing is to clone the people-rest-service project on to your machine. people-rest-service project is the backend api. It's a Java Springboot rest api project, that writes data to mysql database. This api will be consume by a frontend app called people-web-app writen in ReactJS. To clone the project:

```
cd oracle_projects

git clone <url>
```

**Please note that url is the people-rest-service you imported into you own git repo**

Once the project has finished cloning, change into the directory:

```
cd people-service
```

If you are using VisualCode the you can do to open the application in the IDE:

```
code .
```
2. As part of this project, it comes with kubernete descriptors for creating you mysql instance. We are going to start with creating the persistent volume for mysql. While in the project's folder execute the following command. If you are using a different Region other than EU-FRANKFURT. Then:

**Fix and apply correct claim:**
+ Edit the claim file (mysql-volume-claim.yaml)
+ Replace `EU-FRANKFURT-1-AD-1` to the correct one* (such as `UK-LONDON-1-AD-1` if you are using London region)
+ Save


```
kubectl create -f ./k8s/claims/mysql-volume-claim.yaml 
```
Output:
```
persistentvolumeclaim/mysqlclaim created
```

Please check if the volume was create by:

```
kubectl get pvc
```

Output:
```

NAME           STATUS    VOLUME    CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysqlclaim     Bound     ocid1.volume.**   50Gi       RWO            oci            35s

```

*More information and region names can be found in [Creating a Persistent Volume Claim](https://docs.cloud.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengcreatingpersistentvolumeclaim.htm) documentation*

3. Let's now store our mysql password as a secret in your kubernetes cluster. To do this:

```
kubectl create secret generic mysql-pass --from-literal=password=123456

secret/mysql-pass created
```

4. Now that we have our password stored, it's now time to deploy our mysql server on to our cluster. This deployment file will create the mysql pod and the mysql service. To do this:

```
kubectl create -f ./k8s/deployments/mysql-deployment.yaml 
```

Output:
```
deployment.extensions/mysql created
service/mysql created
```

You can check if the pod is running:

```
kubectl get pods
```

Output:
```
NAME                          READY     STATUS    RESTARTS   AGE
mysql-69cfc89647-p42k7        1/1       Running   0          47s
```

You can check if the mysql service is running:

```
kubectl get svc
```
Output:
```
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)     AGE
kubernetes         ClusterIP   10.96.0.1       <none>        443/TCP     180m
mysql              ClusterIP   10.96.136.57    <none>        3306/TCP    1m
```

5. Let now create the **people_db** that is used by the backend api to store data. You will need to connect to the mysql docker container that is running in your cluster and then login to the mysql server and create your database. To connect you will need the name of the pod, in my case **mysql-69cfc89647-p42k7** and use the kubectl exec command as shown below. mysql password is **123456**

```
kubectl exec -it mysql-69cfc89647-p42k7 -- /bin/bash
```
Then:
```

root@mysql-69cfc89647-p42k7:/# mysql -u root -p
Enter password: 

mysql> create database people_db;
Query OK, 1 row affected (0.00 sec)
mysql> exit;

exit
```

---
For additional information on Persistent Storage click [here](https://docs.cloud.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengcreatingpersistentvolumeclaim.htm)

Now you are ready to deploy the Backend API

---
[Container Engine for Kubernetes (OKE) on OCI Workshop Home page](README.md)

[Previous](initial.setup.OKE2)

[Next](deployments.services.OKE4.md)
