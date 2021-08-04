# Compartments
Compartments are logical to manage OCI resources. Details are explained in OCI trainings and [documentation](https://docs.oracle.com/en-us/iaas/Content/Identity/Tasks/managingcompartments.htm).

# Workshops compartment
**emeaccoe** tenancy, is a shared tenancy for workshops. This is applicable only if you are using **emeaccoe** tenancy. 

All of the workshop registrants can use **workshops** compartment as their own. Each user should [create their own compartment](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/creating-cloud-storage-compartment.html), in their names, under workshops compartment and [use it](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/choosingcompartments.htm).

After creating a compartment, it will take up to 2 minutes to have it available to use. OCI Console, caches list of compartments. If you cannot see your compartment within the selection list, refresh your browser.

During the workshop, the resources that you create **MUST** be within the compartment that you have created. While creating the resources (such as compute, network,... any resource). You should assume that compartment as your root.