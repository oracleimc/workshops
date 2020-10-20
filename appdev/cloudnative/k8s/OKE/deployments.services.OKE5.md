# Container Engine for Kubernetes (OKE) on OCI Workshop #

# Prerequisites
+ Make sure you have completed the [Initail Setup](initial.setup.OKE2.md).


# Setup the Oracle Cloud Infrastructure Registry (OCIR) #
In this lab, we are going to setup the Oracle Cloud Infrastructure Registry (OCIR)

+ Login to Oracle Cloud Infrastructure Registry from the Docker CLI
+ Create Docker Registry Secret

### Login to Oracle Cloud Infrastructure Registry from the Docker CLI ###

In a terminal window on the client machine running Docker, log in to Oracle Cloud Infrastructure Registry by entering:

```
docker login {region-key}.ocir.io
```
where {region-key} is the key for the Oracle Cloud Infrastructure Registry region you're using. For example, fra. See the [Availability by Region](https://docs.cloud.oracle.com/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) topic in the Oracle Cloud Infrastructure Registry documentation.

username: {tenancy}/{username}

password: {AuthToken}

For example

```
$ docker login fra.ocir.io
Username: emeaccoe/allenkubai
Password:
Login Succeeded
```
### Create Docker Registry Secret in the Cluster ###

For the kubernetes to be able to pull images from OCIR, we have to added our Auth Token to our cluster as a secret. 

+ Replace {tenancy}/{username} and {Auth Token}

**Please note if you are in another region apart from eu-frankfurt please also change the --docker-server={}, for example for London it will be lhr.ocir.io**

```
kubectl create secret docker-registry ocirsecret --docker-username='{tenancy}/{username}' --docker-password='{Auth Token}' --docker-server=fra.ocir.io --docker-email='api.user@acme.com'
```

Output:

```
secret/ocirsecret created
```

---
[Container Engine for Kubernetes (OKE) on OCI Workshop Home page](README.md)

[Previous](deployments.services.OKE4.md)

[Next](deployments.services.OKE6.md)
