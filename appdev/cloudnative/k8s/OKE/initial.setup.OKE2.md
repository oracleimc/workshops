# Container Engine for Kubernetes (OKE) on OCI Workshop #

Make sure that you have completed all [prerequisites](./prerequisites.md) and [created your Cluster](./create-cluster.OKE1.md).

# Accessing the cloud shell

You will be using the Oracle **OCI Cloud shell** to run the Kubernetes parts of the labs.

Get information how to use [OCI CLoud Shell](../../../../common/cloud-shell.md)

# Configure Helm repository
Helm is the tool we will be using to install standard software into Kubernetes. While it's possible to load software into Kubertetes by hand Helm makes it much easier as it has pre-defined configurations (called charts) that it pulls from an internet based repository.

The OCI Cloud Shell has helm already installed for you, however it does not know what repositories to use for the helm charts. We need to tell helm what repositories to use.

1. Run the following command to add the dashboard repo to helm
```shell
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
```
```
"kubernetes-dashboard" has been added to your repositories
```

2. To can get the current list of repositories run the following command :
```shell
helm repo list
```
```
NAME                    URL
kubernetes-dashboard    https://kubernetes.github.io/dashboard/
```

3. Lastly let's update the helm cache, run the following command :
```shell
helm repo update
```

```
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "kubernetes-dashboard" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

# Getting your cluster access details
Access to the cluster is managed via a config file that by default is located in the `$HOME/.kube` folder, and is called `config`. To check the setup, make sure to have copied your personal kubeconfig file to this location :

1. Create a directory for the Kubernetes config
```shell
mkdir -p $HOME/.kube
```
2. Return to the OCI Web Console, open the "Hamburger" menu on the upper left. Click on the **Developer Services** menu option, then **Kubernetes Clusters (OKE)**
![](./images/oci-console-access-oke.png)
3. Locate your cluster in the list, this will be the one you've been assigned or the one you just created. Click on the name to get to the cluster details.
![](./images/cluster-access-1.png)
4. Click the **Accesss Cluster** button to get the configuration for **your** cluster. This button might be disabled as the cluster is still being initialized (orange). Wait until it becomes available (green)
![](./images/cluster-access-2.png)
5. Make sure that **Cloud Shell Access** is selected and copy the command for the **public endpoint** as we have selected while creating the cluster.
6. Open your OCI Cloud Shell window and **paste** the line to execute it.
```
New config written to the kubeconfig file /home/oracle/.kube/config
```
Note that if there was an existing Kubernetes config file (most likely because you're using an existing tenancy) then the output will say
```
Existing kubeconfig file found at /home/oracle/.kube/config and new config merged into it
```
7. 
    Set the config file to be accessible only by you (This stops warnings from helm about it having the wrong permissions)
```shell
chmod 600 $HOME/.kube/config
```
8. Verify you can access the cluster:
```shell
kubectl get nodes
```
```
NAME        STATUS   ROLES   AGE     VERSION
10.0.10.2   Ready    node    9m16s   v1.16.8
10.0.10.3   Ready    node    9m2s    v1.16.8
```

If the kubectl command returns No resources found. and you have only just created the cluster it may still be initializing the worker nodes (They have to be created, the OS installed and then the Kubernetes software installed before they report ready). Wait a short time and try again until you get the nodes list.  
You should be seeing 3 nodes, same as you have created the cluster.

# Deploy Kubernetes Dashboard
Usually a Kubernetes cluster comes with only the core Kubernetes services installed that are needed to actually run the cluster (e.g. the API, DNS services). Some providers also give you the option of installing other elements, but here we're going to assume you have a minimal cluster with only the core services and will need to setup the other services before you run the rest of the system.

For most standard services in Kubernetes Helm is used to install and configure not just the pods, but also the configuration around them. Helm has templates (called charts) that define how to install potentially multiple services and to set them up.

The latest version of helm is helm 3. This is a client side only program that is used to configure the Kubernetes cluster with the services you chose. If you're familiar with previous versions of helm you will know about the cluster side component "tiller". This is no longer used in Helm 3

Fortunately for us helm 3 is installed within the OCI Cloud Shell, but if later on you want to use your own laptop to manage a Kubernetes cluster here are the instructions for a [local install of helm](https://helm.sh/docs/intro/install/)

## Installing the Kubernetes dashboard
Our first use of helm is to install the kubernetes-dashboard.

Setting up the Kubernetes dashboard (or any) service using helm is pretty easy. it's basically a simple command.

If you are using the OCI Cloud shell for this section of the lab (either in an oracle provided or your own tenancy)

1. To install the dashboard run the following command :
```shell
helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --namespace kube-system --set service.type=LoadBalancer --version 4.0.3
```
```
NAME: kubernetes-dashboard
LAST DEPLOYED: Tue Jun 30 13:07:36 2020
NAMESPACE: kube-system
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
*********************************************************************************
*** PLEASE BE PATIENT: kubernetes-dashboard may take a few minutes to install ***
*********************************************************************************

  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        Watch the status with: 'kubectl get svc -n kube-system -w kubernetes-dashboard'

Get the Kubernetes Dashboard URL by running:
  export SERVICE_IP=$(kubectl get svc -n kube-system kubernetes-dashboard -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
  echo https://$SERVICE_IP/
```
Note that Helm does all the work needed here, it creates the service, deployment, replica set and pods for us and starts things running. Unless you need a very highly customised configuration using helm is way simpler than setting each of these individual elements up yourself.

2. Check the staus of the Helm deployment
```shell
helm list --namespace kube-system
```
```
NAME                    NAMESPACE      REVISION    UPDATED                                 STATUS      CHART                          APP VERSION
kubernetes-dashboard    kube-system    1           2019-12-24 16:16:48.112474 +0000 UTC    deployed    kubernetes-dashboard-2.8.3         2.0.4
```
We've seen it's been deployed by Helm, this doesn't however mean that the pods are actually running yet (they may still be downloading)
3. Check the status of the objects created:
```shell
kubectl get all --namespace kube-system
```
```
NAME                                       READY   STATUS    RESTARTS   AGE
pod/coredns-78f8cf49d4-8pq5c               1/1     Running   0          3d23h
pod/kube-dns-autoscaler-9f6b6c9c9-76tw5    1/1     Running   0          3d23h
pod/kube-flannel-ds-5kn8m                  1/1     Running   1          3d23h
pod/kube-flannel-ds-bqmct                  1/1     Running   1          3d23h
pod/kube-proxy-dlpln                       1/1     Running   0          3d23h
pod/kube-proxy-tzgzp                       1/1     Running   0          3d23h
pod/kubernetes-dashboard-bfdf5fc85-djnvb   1/1     Running   0          66s
pod/proxymux-client-b8cdk                  1/1     Running   0          3d23h
pod/proxymux-client-dnzv8                  1/1     Running   0          3d23h


NAME                           TYPE           CLUSTER-IP     EXTERNAL-IP       PORT(S)                  AGE
service/kube-dns               ClusterIP      10.96.5.5                  53/UDP,53/TCP,9153/TCP   3d23h
service/kubernetes-dashboard   LoadBalancer   10.96.104.87   158.101.177.127   443:32169/TCP            66s

NAME                                          DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR                       AGE
daemonset.apps/kube-flannel-ds                2         2         2       2            2           beta.kubernetes.io/arch=amd64       3d23h
daemonset.apps/kube-proxy                     2         2         2       2            2           beta.kubernetes.io/os=linux         3d23h
daemonset.apps/nvidia-gpu-device-plugin       0         0         0       0            0                                         3d23h
daemonset.apps/nvidia-gpu-device-plugin-1-8   0         0         0       0            0                                         3d23h
daemonset.apps/proxymux-client                2         2         2       2            2           node.info.ds_proxymux_client=true   3d23h

NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/coredns                1/1     1            1           3d23h
deployment.apps/kube-dns-autoscaler    1/1     1            1           3d23h
deployment.apps/kubernetes-dashboard   1/1     1            1           66s

NAME                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/coredns-78f8cf49d4               1         1         1       3d23h
replicaset.apps/kube-dns-autoscaler-9f6b6c9c9    1         1         1       3d23h
replicaset.apps/kubernetes-dashboard-bfdf5fc85   1         1         1       66s
```
We see all the elements of the dashboard: a pod, a replica set, a deployment and a service.

4. Get the list of pods providing the dashboard service by name (the service just goes by it's name, no random identifiers added to it)
```shell
kubectl get pod -n kube-system --selector=app.kubernetes.io/name=kubernetes-dashboard
```
```
NAME                                    READY   STATUS    RESTARTS   AGE
kubernetes-dashboard-bfdf5fc85-djnvb   1/1     Running   0          43m
```

## Accessing the Kubernetes dashboard
First we're going to need create a user to access the dashboard. This involves creating the user, then giving it the kubernetes-dashbaord role that helm created for us when it installed the dashbaord chart.
1. Create admin service acccount:
```shell
kubectl apply -f https://raw.githubusercontent.com/allenkubai/kubernetes/master/oracle/oke/dashboard/oke-admin-service-account.yaml
```
```
serviceaccount/oke-admin created
clusterrolebinding.rbac.authorization.k8s.io/oke-admin created
```
2. Before we can login to the dashboard we need to get the access token for the dashboard-user. We do this using kubectl
```shell
kubectl -n kube-system describe secret `kubectl -n kube-system get secret | grep dashboard-user | awk '{print $1}'`
```
```
Name:         dashboard-user-token-mhtf9
Namespace:    kube-system
Labels:       
Annotations:  kubernetes.io/service-account.name: dashboard-user
              kubernetes.io/service-account.uid: a09cd40c-2663-11ea-a75b-025000000001

Type:  kubernetes.io/service-account-token
Data
====
namespace:  11 bytes
token:      
eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLW1odGY5Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJhMDljZDQwYy0yNjYzLTExZWEtYTc1Yi0wMjUwMDAwMDAwMDEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06YWRtaW4tdXNlciJ9.HUg_9-3HBAG0IJKqCNZvXOS8xdt_n2qO4yNc0Lrh4T4AXnUdMHBR1H8uO6J_GoKSKKeuTJpaIB4Ns4QGaWAvcatFxJWmOywwT6CtbxOeLIyP61PCQju_yfqQO5dTUjNW4O1ciPqAWs6GXL-MRTZdvSiaKvUkD_yOrnmacFxVVZUIKR8Ki4dK0VbxF9VvN_MjZS2YgMz8CghsM6AB3lusqoWOK2SdM5VkIGoAOZzsGMjV2eCYJP3k6qIy2lfOD6KrvERhGZLk8GwEQ7h84dbTa4VHqZurS63fle-esKjtNS5A5Oarez6BReByO6nYwEVQBty3VLt9uKPJ7ZRr1FW5iA

ca.crt:     1025 bytes
```
3. Copy the contents of the token (in this case the eyJh........W5iA text, but it `will` vary in your environment).
4. Save it in a plain text editor on your laptop for easy use later in the lab

As the OCI Cloud Shell runs in a web browser and is not itself a web browser we need to setup access so that the kubernetes-dashboard is available to your web browser on your laptop. This would normally be a problem as it would be running on a network that it internal to the cluster.

Fortunately for us helm is a very powerful mechanism for configuring services, and when we used the helm command to install the dashboard we told it that the service.type was LoadBalancer, this will automatically setup a load balancer for us, making the dashbaord service visible on the public internet, we just need the IP address to use.

5. To get the IP address of the dashboard load balancer run the following command
```shell
kubectl get service kubernetes-dashboard -n kube-system
```
```
NAME                   TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)         AGE
kubernetes-dashboard   LoadBalancer   10.96.21.252   130.61.134.234   443:32302/TCP   4m48s
```
The IP address of the load balancer is in the EXTERNAL-IP column. Note that this can take a few minutes to be assigned, so it it's listed as just re-run the **kubectl get** command after a short while

## Looking around the dashboard
1. Open a web browser and using the IP address you got above and go to:

`https://<load balancer ip address>/#!/login`

In the browser, accept a self signed certificate the mechanism varies by browser and version, but as of August 2020 the following worked.

> In Safari you will be presented with a page saying "This Connection Is Not Private" Click the "Show details" button, then you will see a link titled visit this website click that, then click the Visit Website button on the confirmation pop-up. To update the security settings you may need to enter a password, use Touch ID or confirm using your Apple Watch.

> In Firefox once the security risk page is displayed click on the "Advanced" button, then on the "Accept Risk and Continue" button

> In Chrome once the "Your connection is not private" page is displayed click the advanced button, then you may see a link titled Proceed to ....(unsafe) click that.

We have had reports that some versions of Chrome will not allow you to override the page like this, for Chrome 83 at least one solution is to click in the browser window and type the words `thisisunsafe` (copy and past doesn't seem to work, you need to actually type it). Alternatively use a different browser.

(Of course if you were doing this in a real deployment you's use a real certificate so wouldn't have to worry about a self-signed certificate causing browser problems).

You'll now be presented with the login screen for the dashboard.

2. Click the radio button for the **Token**
3. Enter the token for the admin-user you retrieved earlier
4. Accept to save the password if given the option, it'll make things easier on the next login
5. Press Sign In
![](./images/kubernetes-dashboard.png)

**Important** The Kubernetes dashboard will only keep the login session open for a short time, after which you will be logged out. Unfortunately when your login session expires the Kubernetes dashboard doesn't always return you to the login screen. If you find that you are making changes and the dashboard doesn't reflect them, or that you can see something using kubectl - but not in the dashboard, or you trigger an action on the dashboard (e.g. switching to a different a namespace) but the content doesn't update it's probable that the session has expired. In this case reload the web page or go to the login URL (above), this will reset the pages state and present you with the login screen again, login using your token as previously (the token does not change, so you don't have to extract it again)

### You are now ready to get started. ###

---
[Container Engine for Kubernetes (OKE) on OCI Workshop Home page](README.md)

[Previous](create-cluster.OKE1)

[Next](deployments.storage.OKE3.md)
