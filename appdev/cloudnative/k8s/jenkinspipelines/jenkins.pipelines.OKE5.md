# DevOps: Deploying Microservices to OKE with Jenkins Pipelines (CI/CD) #

# Prerequisites
Make sure you have completed the Initail Setup  [Step 2](jenkins.pipelines.OKE2.md).

# Step 5 - Create Mysql Server in the kubernetes cluster #
In this lab, you are going to set up a mysql server instance in your kubernetes cluster. For this lab, we are going to use the public docker image for mysql. Here are the summary of steps

+ Create a Persistance Volume for mysql
+ Create the mysql instance
+ Create the database in mysql

1. If you have not forked [people-service](https://github.com/allenkubai/people-service.git), please do so. Further on `people-service` will refer to your own forked version of the repository. Clone the `people-service` project on to your machine. `people-service` project is the backend api. It's a Java Springboot rest api project, that writes data to mysql database. This api will be consume by a frontend app called `people-web-app` writen in ReactJS. To clone the project:

```sh
cd oracle_projects

git clone <url> #people-service
```

**Please note that url is the people-service you imported into you own git repo**

Once the project has finished cloning, change into the directory:

```sh
cd people-service
```

2. As part of this project, it comes with kubernete descriptors for creating you mysql instance. We are going to start with creating the persistent volume for mysql. While in the project's folder execute the following command.

```sh
kubectl create -f ./k8s/claims/mysql-volume-claim.yaml 
```
Output:
```sh
persistentvolumeclaim/mysqlclaim created
```

Please check if the volume was create by:

```sh
kubectl get pvc
```

Output:
```sh

NAME           STATUS    VOLUME    CAPACITY   ACCESS MODES   STORAGECLASS   AGE
jenkinsclaim   Bound     ocid1.volume.**   100Gi      RWO            oci            63m
mysqlclaim     Bound     ocid1.volume.**   50Gi       RWO            oci            35s

```
3. Let's now store our mysql password as a secret in your kubernetes cluster. To do this:

```sh
kubectl create secret generic mysql-pass --from-literal=password=123456
```
Output:
```sh
secret/mysql-pass created
```

4. Now that we have our password stored, it's now time to deploy our mysql server on to our cluster. This deployment file will create the mysql pod and the mysql service. To do this:

```sh
kubectl create -f ./k8s/deployments/mysql-deployment.yaml 
```

Output:
```sh
deployment.extensions/mysql created
service/mysql created
```

You can check if the pod is running:

```sh
kubectl get pods
```

Output:
```sh
NAME                          READY     STATUS    RESTARTS   AGE
cd-jenkins-74bbddd846-7k594   1/1       Running   0          140m
mysql-69cfc89647-p42k7        1/1       Running   0          47s
```

You can check if the mysql service is running:

```sh
kubectl get svc
```
Output:
```sh
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)     AGE
cd-jenkins         ClusterIP   10.96.186.250   <none>        8080/TCP    140m
cd-jenkins-agent   ClusterIP   10.96.223.114   <none>        50000/TCP   140m
kubernetes         ClusterIP   10.96.0.1       <none>        443/TCP     180m
mysql              ClusterIP   10.96.136.57    <none>        3306/TCP    1m
```

5. Let now create the **people_db** that is used by the backend api to store data. You will need to connect to the mysql docker container that is running (wait until status in `kubectl get pods` becomes `Running`) in your cluster and then login to the mysql server and create your database. To connect you will need the name of the pod, in my case **mysql-69cfc89647-p42k7** and use the kubectl exec command as shown below. mysql password is **123456**

If you are using `git bash` on Windows, this might not work; use powershell instead
```sh
kubectl exec -it mysql-69cfc89647-p42k7 -- /bin/bash
```
Then:
```sh

root@mysql-69cfc89647-p42k7:/# mysql -u root -p
Enter password: 

mysql> create database people_db;
Query OK, 1 row affected (0.00 sec)
mysql> exit;

exit
```

Now you are ready to deploy the Backend API

---
[Go back to Jenkins Pipelines Workshop Home page](README.md)

[Previous](jenkins.pipelines.OKE4.md)

[Next](jenkins.pipelines.OKE6.md)
