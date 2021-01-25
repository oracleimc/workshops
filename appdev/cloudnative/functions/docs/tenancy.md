# OCI Tenancy
When you sign up for Oracle Cloud Infrastructure, Oracle creates a *tenancy* for your company, which is a secure and isolated partition within Oracle Cloud Infrastructure where you can create, organize, and administer your cloud resources.

In this workshop, you might work with your own tenancy

If this workshop is being delivered by Oracle EMA IMC team, your traininer can provide you an environment (tenancy) for you (explained below)

## EMEA CCoE tenancy
1. Your trainer will provide you to self-registration link
    1. Create your account
    2. Access [OCI Console](https://console.eu-frankfurt-1.oraclecloud.com)
2. Create your own compartment
    1. Identity > Compartments
    2. Select workshops
    3. Press **Create Compartment** button
    4. Fill: Name as your name, description as your full name
    5. Make sure that Parent Compartment is workshops
    6. Press create
    7. For the rest of the workshop, you will work under this compartment as if it is your root compartment in tenancy
3. Refresh
    - New compartment will be created within a minute or two
    - While creating new resources, that compartment might not visible unless page is being refreshed after it's creation (make a small pause before refresh after creating)