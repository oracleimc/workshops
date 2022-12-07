# Lab 2: Configuration
Make sure you have correctly installed required components and using a linux (bash) as stated in [Lab1: Installation](./Lab1-Installation.md)

In this lab, we will be configuring:
- OCI CLI
- Kubernetes cluster
- Kubernetes default namespace
- VBS

Unless stated otherwise, all of the config will happen within remote delveopment environment.

## OCI CLI
1. Craete config folder if necessary:
    ```shell
    mkdir ~/.oci
    ```
2. Open OCI Web Console
3. Go to your use settings > API Keys and press, Add API Key
4. Generate API Key Pair is selected by default. Download both of the files
5. Press Add
6. Copy the content of Configuration File Preview and save it as `~/.oci/config`
7. Move the downloaded key to same folder
8. In the config file, fix the path for key files and save the config file
9. Fix permissions for config files and keys

## Kubernetess cluster
1. [Accessing a Cluster Using kubectl Installed Locally](https://docs.oracle.com/en-us/iaas/Content/ContEng/Concepts/contengaboutaccesscontrol.htm). Your instructor can assis you with that script

    You should be using **Local Access** method from the interface.

    Sample script
    ```shell
    oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.eu-frankfurt-1.aaaaaq --file $HOME/.kube/config --region eu-frankfurt-1 --token-version 2.0.0  --kube-endpoint PUBLIC_ENDPOINT
    export KUBECONFIG=$HOME/.kube/config
    ```

    It is best to add `export KUBECONFIG=$HOME/.kube/config` line to `bashrc`
    ```shell
    echo -e '\nexport KUBECONFIG=$HOME/.kube/config' >> ~/.bashrc
    ```
2. Create your name variable, all as single word and in lowercase
    ```shell
    export YOUR_NAME="yourName"
    ```
    It is recommended to add the same line in your `~/.bashrc` file (modify, if you are different shell)
    ```shell
    echo -e '\nexport YOUR_NAME="yourName"' >> ~/.bashrc
    ```
3. Add rbac role
    ```shell
    kubectl create clusterrolebinding "my-cluster-admin-$YOUR_NAME" --clusterrole=cluster-admin --user=<user_OCID>
    ```
    Give any name for my-cluster-admin-binding. User_ocid is obtained from OCI config file or OCI web console, user settings or from OCI config file (`~/.oci/config`)

4. Create a namespace of your own. Give it your name, a short one without any spaces, all lower case:
    ```shell
    kubectl create namespace $YOUR_NAME
    ```
5. Set this namespace as default
    ```shell
    kubectl config set-context --current --namespace=$YOUR_NAME
    ```

## VBS
1. Open your VBS instance
2. On top right, click to your user picture and click to preferences
3. Under SSH keys, Add key. You will be repating this step 3 and 4, for both local and remote ssh keys.
4. Give an identifier name (local-dev, remove-dev). (For both remote and local) paste the content of public ssh key

---
# Navigation
- Previous: [Lab1: Installation](./Lab1-Installation.md)
- [Home](./README.md)
- Next: [Lab3: Visual Builder Studio](./Lab3-VBS.md)
