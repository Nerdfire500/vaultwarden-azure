# Creates a Vaultwarden Container App within Azurefile external storage

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fadamhnat%2Fvaultwarden-azure%2Fmaster%2Fmain.json)
[![Visualize](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.svg?sanitize=true)](http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fadamhnat%2Fvaultwarden-azure%2Fmaster%2Fmain.json)

This template provides a way to deploy a **Vaultwarden** in a **Azure Container App** with external **file share** and **database** storage that can be used to backup restore data easly.

## Deploy:
1. Click above button and select 
- Resource Group - all resources will be created in that group, you can choose also to create new one
- Storage Account Type - in case that you you like to be more resistant for failure you may choose Standard_GRS or any other storage with redundancy.
- AdminAPI Key - it will be generated automaticly or you can specify your own one. It will be used to access /admin page
- Choose memory and cpu sizing - I recommend to start with 0.25 cpu and 0.5 Memory 
    The total CPU and memory allocations requested for all the containers in a container app must add up to one of the following combinations.
    | CPU | Memory |
    | --- | ---    |
    | 0.25 | 0.5Gi |
    | 0.5 | 1.0Gi |
    | 0.75 | 1.5Gi |
    | 1.0 | 2.0Gi |
    | 1.25 | 2.5Gi |
    | 1.5 | 3.0Gi |
    | 1.75 | 3.5Gi |
    | 2.0 | 4.0Gi |
    ---
- **Database Options**:
  - **Database Option**: Choose between `builtin` (creates a new PostgreSQL server) or `existing` (connects to an existing PostgreSQL server)
  - **For built-in database**: A new Azure Database for PostgreSQL Flexible Server will be created automatically
  - **For existing database**: Provide the following:
    - **Existing Postgres Server Name**: Name of your existing PostgreSQL Flexible Server
    - **Existing Postgres Resource Group**: Resource group containing the server (leave empty to use current resource group)
    - **Database Name**: Name of the database to create on the server (default: vaultwardendb)
    - **Postgres Admin Username**: PostgreSQL administrator username (default: vwadmin)
    - **Important**: When using an existing PostgreSQL server, you must manually create the database specified in "Database Name" on your server if it doesn't already exist. The template will not create the database on external servers.
  - **Db Password**: PostgreSQL administrator password (required for both options)
- **Deploy**

### Creating Database on Existing PostgreSQL Server
If you chose to use an existing PostgreSQL server, you need to create the database manually. Here are a few ways to do it:

**Option 1: Using Azure CLI**
```bash
az postgres flexible-server db create \
  --resource-group <your-resource-group> \
  --server-name <your-server-name> \
  --database-name vaultwardendb
```

**Option 2: Using Azure Portal**
1. Navigate to your PostgreSQL Flexible Server in Azure Portal
2. Select "Databases" from the left menu
3. Click "+ Add" to create a new database
4. Enter the database name (e.g., "vaultwardendb")
5. Click "Save"

**Option 3: Using a PostgreSQL Client**
```sql
CREATE DATABASE vaultwardendb WITH ENCODING 'UTF8' LC_COLLATE='en_US.utf8' LC_CTYPE='en_US.utf8';
```


2. Resource vaultwarden Microsoft.App/containerApps failed - if in some case you will notice failed message, just click **redeploy** and reenter same data as before - it may happen when Azure provision resources and link to storage isn't created at time.

## Updating to new version:
in Azure Portal:
- Open Resource Group -> vaultwarden -> Revision management -> **Create revision** -> type name/suffix -> check vaultwarden in Container image section -> **create**
  This will update your vaultwarden container app into most recent version, keeping data in place, in no downtime.

## Get Admin key:
- Resource Group -> vaultwarden -> Containers -> Environment Variables -> double click on ADMIN_TOKEN **value**
