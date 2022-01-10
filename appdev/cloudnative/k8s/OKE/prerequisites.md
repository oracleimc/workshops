# Prerequisites

1. Create [GitHub](https://github.com/) account, if you do not have one
2. Install required programs
    1. A code editor / IDE. (Such as Sublime, Notepad++ or VS Code). We recommend using [VS Code](https://code.visualstudio.com/download)
    2. Git Client, download if from [git-SCM](https://git-scm.com/download/)
    3. [Oracle Cloud CLI](https://docs.cloud.oracle.com/en-us/iaas/Content/API/SDKDocs/cliinstall.htm) (oci). For Windows, please note that, you need PowerShell (as admin) in order to install (for the rest, we will be using bash terminal). Just install the CLI, **DO NOT** configure it yet.
    4. Install [Node.js](https://nodejs.org/en/download/)
    5. Install **Docker**
        * For [Windows](https://docs.docker.com/docker-for-windows/install/)
        * For [Linux/Mac](https://docs.docker.com/engine/install/)
    6.  Several programs can be installed easly via third party package manager. On Linux/Mac you can use brew, on Windows you can use Chocolatery
        * [Chocolatery](https://chocolatey.org/) (Windows)
            * Kubernetes cli (kubectl). If you have Docker (desktop) installed this is bundled, no need to install.
                ```sh
                choco install kubernetes-cli
                ```
            
        * [Homebrew](https://brew.sh/) (Mac/Linux)
            * Kubernetes cli (kubectl). This might be installed as part of the Docker Desktop app on Mac, please check before proceeding.
                ```sh
                brew install kubectl
                ```
3. If you have not set up [Git ssh](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent), please do so. This gives you the option to clone and push without password.
4. If you are going to use your own tenancy, you need to make sure that correct permissions are in place, check [Own Tenancy](#own-tenancy) section. If you are to use the **emeaccoe** enviornment, please follow instructions of your trainer. Also check information about using [emeaccoe](./emeaccoe.md).


## Using Git & GitHub
After completing the step 3 (Git ssh), you can clone and push repositories via SSH.
On the root of the repository, when you press to the top-right green button, it will display the address. This address might be displayed in HTTPS format. In that case, press to the **Use SSH** button to convert it to SSH format
![](./images/git-using-https.png)

After you have changed it to SSH, you can copy its address and use during clone commands

![](./images/git-using-ssh.png)

Git commands, best works on bash terminals. If you are using Windows, make sure that you are using `Git Bash` only! ![](./images/git-bash.png)


## Own Tenancy
You need to provide necessary permissions for OKE and OCIR to work. A user with necessary privididges (such as admin of the tenancy) can create the policies at the root level. You should **not** be doing this on the shared environment.

![](./images/open-policies.png)

On the left side **List Scope** section, make sure that you are in the root compartment.This policy will add necessary permissions to the service
1. Press `Create policy`
2. Give it a name and description, such as `Container Services`
3. Add Statement: `allow service OKE to manage all-resources in tenancy`
4. Press `Create`

If you want to resrict the access of the OKE with desired `<compartment>`, you can replace the last one to the `allow service OKE to manage all-resources in compartment <compartment>`

The following policies will grant permissions to specific "group" to work. Replace `<group>` with corresponding group name and `<compartment>` with corresponding compartment
1. On `root` level compartment
2. Add policy named `<group>Access`
3. Add Statement: `allow group <group> to manage repos in tenancy`
4. Press `+ Another Statement`, please repeat for each statement
4. Add Statement: `allow group <group> to manage all-resources in compartment <compartment>`

---

[Container Engine for Kubernetes (OKE) on OCI Workshop Home page](README.md)
