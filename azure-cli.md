# Azure CLI (AZ-104)

> 30 of the most used Azure CLI commands for the AZ-104 exam, grouped by exam domain.

## Accounts & Subscriptions

**1. az login**
Signs in to Azure interactively (or with a device code for remote/headless environments).
```bash
az login
az login --use-device-code
```

**2. az account list**
Lists all subscriptions available to the signed-in account.
```bash
az account list --output table
```

**3. az account set**
Sets the active subscription for the current session.
```bash
az account set --subscription "xxxx-xxxx-xxxx-xxxx"
```

**4. az logout**
Signs out of the current Azure CLI session.
```bash
az logout
```

---

## Resource Groups & Governance

**5. az group create**
Creates a new resource group in a specified region.
```bash
az group create --name myRG --location eastus
```

**6. az group list**
Lists all resource groups in the subscription.
```bash
az group list --output table
```

**7. az group delete**
Deletes a resource group and all resources inside it.
```bash
az group delete --name myRG --yes --no-wait
```

**8. az resource list**
Lists resources, optionally filtered by resource group or type.
```bash
az resource list --resource-group myRG --output table
```

**9. az role assignment create**
Assigns an RBAC role to a user, group, or service principal.
```bash
az role assignment create --assignee USER_ID --role "Contributor" --scope /subscriptions/SUB_ID
```

**10. az role assignment list**
Lists role assignments for a user or scope.
```bash
az role assignment list --assignee USER_ID --output table
```

**11. az role definition list**
Lists available role definitions (e.g. Contributor, Reader, Owner).
```bash
az role definition list --name "Contributor"
```

**12. az policy assignment create**
Assigns an Azure Policy definition to a scope.
```bash
az policy assignment create --name "denyRegions" --policy "policyDefinitionId" --scope "/subscriptions/SUB_ID"
```

---

## Identity (Azure AD / Entra ID)

**13. az ad user create**
Creates a new Azure AD (Entra ID) user.
```bash
az ad user create --display-name "John Doe" --user-principal-name "john@contoso.com" --password "P@ssw0rd123"
```

**14. az ad user list**
Lists or searches Azure AD users.
```bash
az ad user list --display-name "John Doe"
```

**15. az ad group create**
Creates a new Azure AD security group.
```bash
az ad group create --display-name "IT Admins" --mail-nickname "itadmins"
```

**16. az ad group member add**
Adds a member to an Azure AD group.
```bash
az ad group member add --group "IT Admins" --member-id USER_OBJECT_ID
```

---

## Storage

**17. az storage account create**
Creates a new storage account.
```bash
az storage account create --name mystorageacct --resource-group myRG --location eastus --sku Standard_LRS
```

**18. az storage account show**
Retrieves properties of a storage account.
```bash
az storage account show --name mystorageacct --resource-group myRG
```

**19. az storage container create**
Creates a blob container inside a storage account.
```bash
az storage container create --name mycontainer --account-name mystorageacct
```

**20. az storage blob upload**
Uploads a file as a blob to a container.
```bash
az storage blob upload --container-name mycontainer --file ./data.txt --name data.txt --account-name mystorageacct
```

**21. az storage blob list**
Lists blobs inside a container.
```bash
az storage blob list --container-name mycontainer --account-name mystorageacct --output table
```

---

## Virtual Machines & Compute

**22. az vm create**
Creates a new virtual machine.
```bash
az vm create --resource-group myRG --name myVM --image Win2019Datacenter --admin-username azureuser --admin-password "P@ssw0rd123"
```

**23. az vm list**
Lists VMs, optionally filtered by resource group.
```bash
az vm list --resource-group myRG --output table
```

**24. az vm start / az vm stop**
Starts or stops (deallocates) a virtual machine.
```bash
az vm start --resource-group myRG --name myVM
az vm stop --resource-group myRG --name myVM
```

**25. az vm restart**
Restarts a running virtual machine.
```bash
az vm restart --resource-group myRG --name myVM
```

**26. az vm delete**
Deletes a virtual machine.
```bash
az vm delete --resource-group myRG --name myVM --yes
```

**27. az disk list / az vm disk attach**
Creates and attaches a managed disk to a VM.
```bash
az vm disk attach --resource-group myRG --vm-name myVM --name myDisk --new --size-gb 128
az disk list --resource-group myRG --output table
```

---

## Networking

**28. az network vnet create**
Creates a new virtual network (VNet).
```bash
az network vnet create --resource-group myRG --name myVNet --address-prefix 10.0.0.0/16
```

**29. az network public-ip create**
Creates a public IP address resource.
```bash
az network public-ip create --resource-group myRG --name myPublicIP --sku Standard
```

**30. az network lb create**
Creates a load balancer to distribute traffic across VMs.
```bash
az network lb create --resource-group myRG --name myLB --sku Standard --public-ip-address myPublicIP
```

---

## Bonus: DNS, Bastion & Monitoring
* Create a DNS zone: `az network dns zone create --resource-group myRG --name contoso.com`
* Add a DNS A record: `az network dns record-set a add-record --resource-group myRG --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4`
* Test IP flow (NSG troubleshooting): `az network watcher test-ip-flow --resource-group myRG --vm myVM --direction Inbound --protocol TCP --local 10.0.0.4:80 --remote 1.2.3.4:*`
* Create an action group (alerts): `az monitor action-group create --resource-group myRG --name "EmailAdmins" --short-name "email" --action email admin admin@contoso.com`
* Create a Recovery Services vault (backup): `az backup vault create --resource-group myRG --name myVault --location eastus`

---

## Quick Reference Table
| # | Command | Purpose |
|---|---|---|
| 1 | `az login` | Sign in to Azure |
| 2 | `az account list` | List subscriptions |
| 3 | `az account set` | Set active subscription |
| 4 | `az logout` | Sign out |
| 5 | `az group create` | Create resource group |
| 6 | `az group list` | List resource groups |
| 7 | `az group delete` | Delete resource group |
| 8 | `az resource list` | List resources |
| 9 | `az role assignment create` | Assign RBAC role |
| 10 | `az role assignment list` | List role assignments |
| 11 | `az role definition list` | List role definitions |
| 12 | `az policy assignment create` | Assign a policy |
| 13 | `az ad user create` | Create Azure AD user |
| 14 | `az ad user list` | List/search Azure AD users |
| 15 | `az ad group create` | Create Azure AD group |
| 16 | `az ad group member add` | Add user to group |
| 17 | `az storage account create` | Create storage account |
| 18 | `az storage account show` | Get storage account |
| 19 | `az storage container create` | Create blob container |
| 20 | `az storage blob upload` | Upload blob |
| 21 | `az storage blob list` | List blobs |
| 22 | `az vm create` | Create VM |
| 23 | `az vm list` | List VMs |
| 24 | `az vm start` / `az vm stop` | Start/stop VM |
| 25 | `az vm restart` | Restart VM |
| 26 | `az vm delete` | Delete VM |
| 27 | `az vm disk attach` | Attach managed disk |
| 28 | `az network vnet create` | Create VNet |
| 29 | `az network public-ip create` | Create public IP |
| 30 | `az network lb create` | Create load balancer |
