# Container Engine for Kubernetes (OKE) on OCI Workshop #

# Prerequisites
Make sure you have completed the [Initail Setup](initial.setup.OKE2.md)

# Create an Ingress Controller (Load Balancer) for the kubernetes cluster #
In this lab, you are going to set up an Ingress for the applications. Ingress allows external access to services within the Kubernetes cluster. In our case, our we are going to create an nginx-ingress-controller which will be Load Balance that will be shared by the FrontEnd application and by the Backend application.

+ Create the Load Balancer
+ Verify the Load Balancer was created

1. The first thing you need to do is create a cluster role binding using the user used to create the cluster. Use the command below. **Do not forget to replace the --user= with your OCID**

```
kubectl create clusterrolebinding my-cluster-admin-binding --clusterrole=cluster-admin --user=ocid1.user.oc1..xxxxx
```
Output:
```
clusterrolebinding.rbac.authorization.k8s.io/my-cluster-admin-binding created
```

2. For you to create the Ingress Controller, Run the following command

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.3/deploy/static/provider/cloud/deploy.yaml

```

For more information on how to create an Ingress controller on OKE go [here](https://docs.cloud.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengsettingupingresscontroller.htm) 

3. Check if the your ingress-nginx is up check the running services, using the command:

```
kubectl get svc -n ingress-nginx
```

Result:

```
NAME                                 TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.96.180.127   130.162.243.82   80:31593/TCP,443:31499/TCP   24m
ingress-nginx-controller-admission   ClusterIP      10.96.86.9      <none>           443/TCP                      24m
```

*if you see Pending under EXTERNAL-IP, just repeat the command*

**Copy the IP and place somewhere for later use**

4. Now check if you can see you load balancer on your OCI console.

![](./images/kube-ingress-2.png)

---
[Container Engine for Kubernetes (OKE) on OCI Workshop Home page](README.md)

[Previous](deployments.storage.OKE3.md)

[Next](deployments.services.OKE5.md)
