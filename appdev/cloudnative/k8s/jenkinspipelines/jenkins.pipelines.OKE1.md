# Create your own cluster
1. Navigate to Menu > Developer Services > Kubernetes Clusters
2. From the left side, select target compartment  
![](./images/select-compartment.png)
3. Make sure you are in desired region  
![](./images/change-region.png)
1. Follow the [Guide](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/index.html)
You can create using the Quick create option.
    1. Select Quick Create ![](./images/oke-create-1.png)
    2. Give a name to your cluster
    3. Kubernetes version = latest
    4. Select Visiblity=Public
    5. Shape=VM.Standard2.1
    6. Number of Nodes = 3  
    ![](./images/oke-create-2.png)
    7. Next & Create Cluster
    8. This workflow dialog will quickly complete. Howerver this will not finish creating it. It will take some time to fully create the cluster  
    ![](./images/oke-create-3.png)
    9. When it is fully created, the state will be updated automatically, no need to refresh the page.  
    ![](./images/oke-create-4.png)
    10. After seeing Active state, you can proceed with next steps

[Go back to Jenkins Pipelines Workshop Home page](README.md)

[Next](jenkins.pipelines.OKE2.md)

