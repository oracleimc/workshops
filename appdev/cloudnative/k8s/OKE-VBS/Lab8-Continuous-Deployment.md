# Continuous Deployment
In this lab we will make CD flow for Web App:
- Make build whenever a change in `master` branch
- Roll out updates via VBS build task
- Create pipeline

## Reconfigure build-people-web-app task
![Reconfigure build-people-web-app](./images/8-configure-build.png)

Go to `Git` tab and select `Automatically perform build on SCM commit`

![Automatically perform build on SCM commit](./images/8-automatically-perform-build-on-scm-commit.png)

Save!

After any commit is added within `master` branch will trigger an automatic build

## Create update task
Create a new build, named `update-people-web-app` and select the same build template.

1. Go to steps, add `OCIcli`
2. In order to fill this step, more information is needed. Check your OCI config file.
    ```shell
    cat ~/.oci/config
    ```
    This will give you something like:
    ```
    user=ocid1.user.oc1..aaaamqb33a
    fingerprint=10:e1:ea:39:75:a5:78:2c:4f:12:3f:dd:2b:64:4a:1b
    key_file=/home/opc/.oci/oci_api_key.pem
    tenancy=ocid1.tenancy.oc1..aaaaslvibj4a
    region=eu-frankfurt-1
    ```
    Also you need content of the `key_file`. Use the part stated in the config file
    ```shell
    cat /home/opc/.oci/oci_api_key.pem
    ```
    ```
    -----BEGIN RSA PRIVATE KEY-----
    MIIEogIBAAKCAQEAvpJcOYZN+FgnD/tLsiKsIEOOCIbAGQNlrX9H2GRC0sTluqMR
    ................................................................
    ................................................................
    ................................................................
    ................................................................
    fp6LMQ83EYW3IPQ1EbLembKQ8jVOWPDhHVhrZpP30zsY7ynH8LY=
    -----END RSA PRIVATE KEY-----
    ```
    Use the data to fill the step. `Passphrase` could be empty (if applicable)
3. Add `Unix Shell` step
4. Write the following script, change values as needed!:
    ```shell
    oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.eu-frankfurt-1.aaaaaq --file $HOME/.kube/config --region eu-frankfurt-1 --token-version 2.0.0  --kube-endpoint PUBLIC_ENDPOINT
    export KUBECONFIG=$HOME/.kube/config
    kubectl config set-context --current --namespace=yourname
    kubectl rollout restart deployment/people-web-app
    ```
    > You should be setting correct cluster configuration command  
    > You should be using same namespace that you have been using all along

## Create Pipeline
1. Go to `Build` > `Pipelines` within VBS
2. Create Pileline
3. Name it: `auto-deploy-web-app`, leave `Auto start when pipeline jobs are built externally` selected  
   ![Create Pipeline](./images/8-create-pipeline.png)
4. Drag and drop `build-people-web-app` and `update-people-web-app` into canvas. Draw the arrows as seen in the picture.  
   ![Pipeline flow](./images/8-pipeline-flow.png)
5. Save

## Perform change in the code
While inside `people-web-app`:
```shell
code src/common/AppHeader.css
```
Add `background-color: #666666;` line to `.app-header` css class
```css
  .app-header {
    position: fixed;
    width: 100%;
    box-shadow: 0 2px 8px #f0f1f2;
    z-index: 10;
    padding: 0;
    background-color: #CCCCCC
  }
```
After modification, execute the following:
```shell
git add src/common/AppHeader.css
git commit -m "light gray header color"
git push
```

Right after `git push`command, go to your `Builds` section into VBS, and watch it finish. You can switch between Jobs and Pipeline.

When the builds are completed, refresh the page of your applicaiton and observe the change.

# The End
This concludes this workshop. Happy OKE!

---
# Navigation
- Previous: [Lab7: Build & Deploy People App](./Lab7-Build-Deploy-People-App.md)
- [Home](./README.md)