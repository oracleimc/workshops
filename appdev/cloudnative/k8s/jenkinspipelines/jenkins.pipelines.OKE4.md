# DevOps: Deploying Microservices to OKE with Jenkins Pipelines (CI/CD) #

**Step 3** - *Assumption* [Create Jenkins Instance in your kubernetes cluster](./jenkins.pipelines.OKE3.md) is complete.

# Prerequisites
Make sure you are logged in to your jenkins instance on your kubernetes cluster. If you are having trouble, please refer to [Step 3](./jenkins.pipelines.OKE3.md).

# Step 4 - Configure the Jenkins instance #
In this lab, you are going to configure the jenkins instance. At the end of the lab you should have setup:

+ Created credentials using the kubeconfig file
+ Created credentials for OCIR (Oracle Cloud Infrastructure Registry) - *This is where will store and pull our docker images from.*
+ Add the kubernetes credentials to the kubernetes plugin in our jenkins instances

1. On the main dashboard, please click on *Credentials > System > Global credentials* as shown on the screenshot below:

![](./images/jenkins-adding-creds-1.png)

2. On the next screen click on *Add Credentials*. The first credentials we are going to add is the OCI-config file you downloaded in the initial setup step. Please input as shown in the screenshot below then click **OK**:

+ Kind - **Secret file**
+ File - **Choose the config file from you machine**
+ ID - **oci-kubernetes** *This ID is very important. You will need it in the of the Jenkinfile in later steps*
+ Description - **oci kubernetes** *You can write your own description*

![](./images/jenkins-adding-creds-kube-config-1.png)

3. Next, click on *Add Credentials* again. This time we are going to add **OCIR** credentials. Please input as shown below replace username and password with your own values:

+ Kind - **Username with password**
+ Username - **{tenancy/username}**. *For me it's gse00013828 from my tenancy and api.user for my user. Therefore username should be gse00013828/api.user.*
+ password - **Auth Token** *This was generated during the initial setup*
ID - **ocir-credentials** *This ID is very important. You will need it in the of the Jenkinfile in later steps*
+ Description - **ocir credentials** *You can write your own description*

![](./images/jenkins-adding-creds-ocir-config-1.png)

4.Now that we have a credentials setup, we can now add our kubernetes credentials to our kubernetes plugin in our jenkins instance. To do this, Click on *Manage Jenkins > Configure System* as shown in the screenshot below:

![](./images/jenkins-adding-creds-to-kube-plugin-1.png)

5. On the configure page, scroll down to the *Kubernetes* plugin. Under credentials, select your *oci kubernetes* then click *Test Connection*. Once it succesfull, scroll to the bottom of the page and click *Save > Apply*

![](./images/jenkins-adding-creds-to-kube-plugin-2.png)

**Now your jenkins instance is ready to execute pipelines on to your kubernetes clusters**

---
[Go back to Jenkins Pipelines Workshop Home page](README.md)

[Previous](jenkins.pipelines.OKE3.md)

[Next](jenkins.pipelines.OKE5.md)



    
