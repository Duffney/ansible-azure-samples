# ansible-on-azure

Sample Ansible playbooks for deploying and managing Azure resources.

## Prerequisites

* [Docker Desktop]()
* [PowerShell]() + [Azure PowerShell]() OR [AzCLI]()

## 1. Create an Azure AD Service Principal

### PowerShell

1. Create the Azure AD Service Principal Account

    ```powershell
    $password = '<Password>'

    $credentials = New-Object Microsoft.Azure.Commands.ActiveDirectory.PSADPasswordCredential `
    -Property @{ StartDate=Get-Date; EndDate=Get-Date -Year 2024; Password=$password}

    $spSplat = @{
      DisplayName = 'ansible'
      PasswordCredential = $credentials
    }

    $sp = New-AzAdServicePrincipal @spSplat
    ```

    Replace `<Password>` with your password.

2. Assign the Contributor Role to the Service Principal

    ```powershell
    $subId = (Get-AzContext).Subscription.Id
    
    $roleAssignmentSplat = @{
      ObjectId = $sp.id;
      RoleDefinitionName = 'Contributor';
      Scope = "/subscriptions/$subId"
    }
    
    New-AzRoleAssignment @roleAssignmentSplat
    ```

    > NOTE: To improve security, change the scope of the role assignment to a resource group instead of a subscription.

### AzCLI

```azurecli

```

## 2. Build the Ansible Container

**Build the Ansible container image.**

Change to the `\ansible-container\$version` directory, `$version` represents the version of Ansible you want to run.

```powershell
docker build . -t ansible
```

## 3. Run the Ansible Container

### Output the required Azure AD Service Principal information

```powershell
@{
    subscriptionId = (Get-AzContext).Subscription.Id
    clientid = (Get-AzADServicePrincipal -DisplayName 'ansible').ApplicationId.Guid
    tenantid = (Get-AzContext).Tenant.Id
}
```

### Run the Container

Windows

```powershell
docker run -it --rm --volume ${PWD}:/ansible --workdir /ansible `
--env "AZURE_SUBSCRIPTION_ID=<Azure_Subscription_ID>" `
--env "AZURE_CLIENT_ID=<Service_Principal_Application_ID>" `
--env "AZURE_SECRET=<Service_Principal_Password>" `
--env "AZURE_TENANT=<Azure_Tenant>" `
ansible
```

Linux

```bash
docker run -it --rm --volume "$(pwd):/ansible" --workdir /ansible \
--env "AZURE_SUBSCRIPTION_ID=<Azure_Subscription_ID>" \
--env "AZURE_CLIENT_ID=<Service_Principal_Application_ID>" \
--env "AZURE_SECRET=<Service_Principal_Password>" \
--env "AZURE_TENANT=<Azure_Tenant>" \
ansible
```

Replace `<Values>` with the information output in step 3.
