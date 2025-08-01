---
title: Create a virtual machine on a virtual network
description: Learn how to create virtual machines (VM) connected to a virtual network (VNet) with the Azure CLI.
ms.topic: tutorial
ms.service: azure-cli
ms.custom: devx-track-azurecli
keywords: azure cli create vm, virtual machine in azure cli
---

# Create a virtual machine on a virtual network

Virtual machines (VM) in Azure have a large number of dependencies. The CLI creates these resources
for you based on the command-line arguments you specify. In this section, you learn how to deploy a
VM to a virtual network (VNet).

To deploy a VM on a VNet, both must be in the same Azure location. You can't change the VNet the VM
connects to after creating it.

## Create a VM

Use the [az vm create][01] command to create a new virtual machine running Ubuntu. This virtual
machine uses SSH authentication for sign in, and is connected to the subnet and VNet you created in
the previous section. In PowerShell, declare variables with a "$", and use quotes for values, such
as $vmName = "TutorialVM1".

# [Bash](#tab/bash)

```azurecli-interactive
# create Bash shell variable
vmName=TutorialVM1

az vm create \
  --resource-group $resourceGroup \
  --name $vmName \
  --image Ubuntu2204 \
  --vnet-name $vnetName \
  --subnet $subnetName \
  --generate-ssh-keys \
  --output json \
  --verbose 
```

# [PowerShell](#tab/powershell)

```azurecli-interactive
# Create PowerShell variable
$vmName = "TutorialVM1"

az vm create `
    --resource-group $resourceGroup `
    --name $vmName `
    --image Ubuntu2204 `
    --vnet-name $vnetName `
    --subnet $subnetName `
    --generate-ssh-keys `
    --output json `
    --verbose
```

---

> [!NOTE]
> If you have an SSH key named `id_rsa` already available, this key is used for authentication
> rather than having a new key generated.

As the VM is created, you see the local values used and Azure resources being created due to the
`--verbose` option. Once the VM is ready, a JSON is returned from the Azure service including the
public IP address.

```output
{
  "fqdns": "",
  "id": "...",
  "location": "eastus",
  "macAddress": "...",
  "powerState": "VM running",
  "privateIpAddress": "...",
  "publicIpAddress": "<PUBLIC_IP_ADDRESS>",
  "resourceGroup": "TutorialResources",
  "zones": ""
}
```

Confirm that the VM is running by connecting over SSH.

```bash
ssh <PUBLIC_IP_ADDRESS>
```

Go ahead and sign out from the VM by typing `exit`.

There are alternative methods to obtain this IP address after the VM starts. In the next section,
you'll see how to get detailed information on the VM, and how to filter it.

<!-- link references -->

[01]: /cli/azure/vm#az_vm_create
