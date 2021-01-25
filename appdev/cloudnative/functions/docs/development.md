# Function Development Environment

## Software
In order to develop Oracle Functions, you need the following software:
- [Fn CLI](https://github.com/fnproject/fn#install-cli-tool)
    - Fn cli is partially supported on Windows. It is recommended to use a Linux or Mac. If you are using a Windows machine, using a development enviornment on OCI is explained [below](#using-a-development-environment-on-oci)
- [OCI CLI](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/cliinstall.htm)
- [Docker](https://docs.docker.com/engine/install/)
- Text Editor / IDE
    - In this workshop we picked [Visual Studio Code](https://code.visualstudio.com/download). Using other IDE is up to you
- [Git](https://git-scm.com/downloads)

## Test run
Assuming you have installed all, let's make a test run:
1. Test git
    ```shell
    $ git status
    ```
    response:
    ```shell
    > fatal: not a git repository (or any of the parent directories): .git
    ```
    Getting this error is expected
2. Docker
    ```shell
    $ docker ps
    ```
    response, it will display running containers, similar to this:
    ```shell
    > CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    ```
    If you need to `sudo docker`, means you have a missing post installation step. Follow the [Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/) guide
3. OCI
    ```shell
    $ oci iam compartment list
    ```
    Expected response is a JSON output. If you get a different result, error, than you need to finish configuring OCI:
    ```shell
    $ oci setup config
    ```
    - [Where to Get the Tenancy's OCID and User's OCID](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/apisigningkey.htm#five)
    - [Regions and Availability Domains](https://docs.oracle.com/en-us/iaas/Content/General/Concepts/regions.htm#top)
    - [Required Keys and OCIDs](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/apisigningkey.htm)
    
    After configuring OCI, retry first command
4. Fn
    ```shell
    fn --version
    ```
    response:
    ```shell
    > fn version 0.6.1
    ```
    Version could be equal or greater than this



## Using a development environment on OCI
This step is **optional** for Linux or Mac users, it is recommended for Windows user.

If you have decided to use a development environment on OCI, you do not need to install the software to your local machine, except Visual Studio Code.

1. Follow the steps [Development Setup on OCI](https://blogs.oracle.com/imc/development-on-oci) blog
2. Install required the software, mentioned above

