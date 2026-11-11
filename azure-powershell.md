# Azure PowerShell (AZ-104)

> 30 of the most used Az PowerShell cmdlets for the AZ-104 exam, grouped by exam domain.

## Accounts & Subscriptions

**1. Connect-AzAccount**
Signs in to Azure with an interactive login.
```powershell
Connect-AzAccount
```

**2. Get-AzSubscription**
Lists all subscriptions available to the signed-in account.
```powershell
Get-AzSubscription
```

**3. Set-AzContext**
Sets the active subscription/context for the current session.
```powershell
Set-AzContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
```

**4. Disconnect-AzAccount**
Signs out of the current Azure session.
```powershell
Disconnect-AzAccount
```

---

## Resource Groups & Governance

**5. New-AzResourceGroup**
Creates a new resource group in a specified region.
```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

**6. Get-AzResourceGroup**
Retrieves one or all resource groups in the subscription.
```powershell
Get-AzResourceGroup -Name "myResourceGroup"
```

**7. Remove-AzResourceGroup**
Deletes a resource group and all resources inside it.
```powershell
Remove-AzResourceGroup -Name "myResourceGroup" -Force
```

**8. Get-AzResource**
Lists resources, optionally filtered by resource group or type.
```powershell
Get-AzResource -ResourceGroupName "myResourceGroup"
```

**9. New-AzRoleAssignment**
Assigns an RBAC role to a user, group, or service principal.
```powershell
New-AzRoleAssignment -SignInName "user@contoso.com" -RoleDefinitionName "Contributor" -ResourceGroupName "myResourceGroup"
```

**10. Get-AzRoleAssignment**
Lists role assignments for a user, group, or scope.
```powershell
Get-AzRoleAssignment -ResourceGroupName "myResourceGroup"
```

**11. New-AzPolicyDefinition**
Creates a custom Azure Policy definition.
```powershell
New-AzPolicyDefinition -Name "denyRegions" -DisplayName "Deny specific regions" -Policy "policy.json"
```

**12. New-AzPolicyAssignment**
Assigns a policy definition to a scope (subscription, resource group).
```powershell
New-AzPolicyAssignment -Name "denyRegionsAssignment" -PolicyDefinition $definition -Scope "/subscriptions/xxxx"
```

---

## Identity (Azure AD / Entra ID)

**13. New-AzADUser**
Creates a new Azure AD (Entra ID) user.
```powershell
New-AzADUser -DisplayName "John Doe" -UserPrincipalName "john@contoso.com" -Password $securePassword -MailNickname "john"
```

**14. Get-AzADUser**
Retrieves information about Azure AD users.
```powershell
Get-AzADUser -UserPrincipalName "john@contoso.com"
```

**15. New-AzADGroup**
Creates a new Azure AD security group.
```powershell
New-AzADGroup -DisplayName "IT Admins" -MailNickname "itadmins"
```

**16. Add-AzADGroupMember**
Adds a member to an Azure AD group.
```powershell
Add-AzADGroupMember -TargetGroupObjectId $groupId -MemberObjectId $userId
```

---

## Storage

**17. New-AzStorageAccount**
Creates a new storage account.
```powershell
New-AzStorageAccount -ResourceGroupName "myResourceGroup" -Name "mystorageacct" -Location "EastUS" -SkuName "Standard_LRS"
```

**18. Get-AzStorageAccount**
Retrieves properties of a storage account.
```powershell
Get-AzStorageAccount -ResourceGroupName "myResourceGroup" -Name "mystorageacct"
```

**19. New-AzStorageContainer**
Creates a blob container inside a storage account.
```powershell
New-AzStorageContainer -Name "mycontainer" -Context $ctx -Permission Blob
```

**20. Set-AzStorageBlobContent**
Uploads a file as a blob to a container.
```powershell
Set-AzStorageBlobContent -File "./data.txt" -Container "mycontainer" -Blob "data.txt" -Context $ctx
```

**21. New-AzRoleAssignment (Storage scope)**
Grants a role scoped to a storage account (e.g. Storage Blob Data Contributor).
```powershell
New-AzRoleAssignment -SignInName "user@contoso.com" -RoleDefinitionName "Storage Blob Data Contributor" -Scope $storageAccountId
```

---

## Virtual Machines & Compute

**22. New-AzVM**
Creates a new virtual machine.
```powershell
New-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM" -Location "EastUS" -Image "Win2019Datacenter" -Credential $cred
```

**23. Get-AzVM**
Retrieves information about one or more VMs.
```powershell
Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

**24. Start-AzVM / Stop-AzVM**
Starts or stops (deallocates) a virtual machine.
```powershell
Start-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
Stop-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM" -Force
```

**25. Restart-AzVM**
Restarts a running virtual machine.
```powershell
Restart-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

**26. Remove-AzVM**
Deletes a virtual machine (does not remove attached disks/NICs by default).
```powershell
Remove-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM" -Force
```

**27. New-AzDisk / Add-AzVMDataDisk**
Creates a managed disk and attaches it to a VM.
```powershell
$diskConfig = New-AzDiskConfig -Location "EastUS" -CreateOption Empty -DiskSizeGB 128
New-AzDisk -ResourceGroupName "myResourceGroup" -DiskName "myDataDisk" -Disk $diskConfig
```

---

## Networking

**28. New-AzVirtualNetwork**
Creates a new virtual network (VNet).
```powershell
New-AzVirtualNetwork -ResourceGroupName "myResourceGroup" -Name "myVNet" -AddressPrefix "10.0.0.0/16" -Location "EastUS"
```

**29. New-AzPublicIpAddress**
Creates a public IP address resource for use with a VM or load balancer.
```powershell
New-AzPublicIpAddress -ResourceGroupName "myResourceGroup" -Name "myPublicIP" -AllocationMethod Static -Location "EastUS"
```

**30. New-AzLoadBalancer**
Creates a load balancer to distribute traffic across VMs.
```powershell
New-AzLoadBalancer -ResourceGroupName "myResourceGroup" -Name "myLB" -Location "EastUS" -Sku "Standard" -FrontendIpConfiguration $feConfig -BackendAddressPool $beConfig
```

---

## Quick Reference Table
| # | Cmdlet | Purpose |
|---|---|---|
| 1 | `Connect-AzAccount` | Sign in to Azure |
| 2 | `Get-AzSubscription` | List subscriptions |
| 3 | `Set-AzContext` | Set active subscription |
| 4 | `Disconnect-AzAccount` | Sign out |
| 5 | `New-AzResourceGroup` | Create resource group |
| 6 | `Get-AzResourceGroup` | Get resource group(s) |
| 7 | `Remove-AzResourceGroup` | Delete resource group |
| 8 | `Get-AzResource` | List resources |
| 9 | `New-AzRoleAssignment` | Assign RBAC role |
| 10 | `Get-AzRoleAssignment` | List role assignments |
| 11 | `New-AzPolicyDefinition` | Create policy definition |
| 12 | `New-AzPolicyAssignment` | Assign a policy |
| 13 | `New-AzADUser` | Create Azure AD user |
| 14 | `Get-AzADUser` | Get Azure AD user |
| 15 | `New-AzADGroup` | Create Azure AD group |
| 16 | `Add-AzADGroupMember` | Add user to group |
| 17 | `New-AzStorageAccount` | Create storage account |
| 18 | `Get-AzStorageAccount` | Get storage account |
| 19 | `New-AzStorageContainer` | Create blob container |
| 20 | `Set-AzStorageBlobContent` | Upload blob |
| 21 | `New-AzRoleAssignment` (storage) | Grant storage role |
| 22 | `New-AzVM` | Create VM |
| 23 | `Get-AzVM` | Get VM info |
| 24 | `Start-AzVM` / `Stop-AzVM` | Start/stop VM |
| 25 | `Restart-AzVM` | Restart VM |
| 26 | `Remove-AzVM` | Delete VM |
| 27 | `New-AzDisk` | Create managed disk |
| 28 | `New-AzVirtualNetwork` | Create VNet |
| 29 | `New-AzPublicIpAddress` | Create public IP |
| 30 | `New-AzLoadBalancer` | Create load balancer |
