# DevOps: Deploying Microservices to OKE with Jenkins Pipelines (CI/CD) #

# Prerequisites
Make sure you have completed the Initail Setup  [Step 2](jenkins.pipelines.OKE2.md).

# Step 6 - Create an Ingress Controller for the kubernetes cluster #
In this lab, you are going to set up an Ingress for the applications. Ingress allows external access to services within the Kubernetes cluster. In our case, our we are going to create an nginx-ingress-controller which will be Load Balance that will be shared by the FrontEnd application and by the Backend application.

+ Create the Load Balancer
+ Verify the Load Balancer was created

1. The first thing you need to do is create a cluster role binding using the user used to create the cluster. Use the command below. **Do not forget to replace the --user= with your OCID**

```sh
kubectl create clusterrolebinding my-cluster-admin-binding --clusterrole=cluster-admin --user=ocid1.user.oc1..xxxxx
```
Output:
```sh
clusterrolebinding.rbac.authorization.k8s.io/my-cluster-admin-binding created
```

2. For you to create the Ingress Controller, Run the following command

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.30.0/deploy/static/mandatory.yaml
```

3. Next we are going to define our ingress-nginx ingress controller as a load balancer service: This file can be found under: https://github.com/allenkubai/kubernetes/tree/master/oracle/oke/shared-ingress/cloud-generic.yaml

```yaml
kind: Service
apiVersion: v1
metadata:
  name: ingress-nginx
  namespace: ingress-nginx
  labels:
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
spec:
  type: LoadBalancer
  selector:
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
  ports:
    - name: http
      port: 80
      targetPort: http
```

To apply the deployment:

```sh
cd oracle_projects/kubernetes
kubectl apply -f ./oracle/oke/shared-ingress/cloud-generic.yaml
```
For more information on how to create an Ingress controller on OKE go [here](https://docs.cloud.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengsettingupingresscontroller.htm) 

2. Check if the your ingress-nginx is up check the running services, using the command:

```sh
kubectl get svc -n ingress-nginx
```

Result:

```sh
NAME            TYPE           CLUSTER-IP     EXTERNAL-IP         PORT(S)                       AGE
ingress-nginx   LoadBalancer   10.96.13.245   130.61.198.159      80:30756/TCP,443:30118/TCP    1h
```

*if you see Pending under EXTERNAL-IP, just repeat the command until it is resolved*

3. Now check if you can see you load balancer on your OCI console.

![](./images/kube-ingress-2.png)

---
[Go back to Jenkins Pipelines Workshop Home page](README.md)

[Previous](jenkins.pipelines.OKE5.md)

[Next](jenkins.pipelines.OKE7.md)
