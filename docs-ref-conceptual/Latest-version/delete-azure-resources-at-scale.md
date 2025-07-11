---
title: How to delete resources at scale using the Azure CLI
description: Learn how to delete multiple Azure resources from a script and log progress to a file.
ms.service: azure-cli
ms.custom: devx-track-azurecli
#customer intent: As an Azure resource manager, I want to create Azure resources at scale using a script. I want to log progress to a local TXT file so I don't have to sort through my Windows log for results.
---

# How to delete resources at scale using the Azure CLI

As an Azure cloud resource administrator, you often have to delete several Azure resources when
removing an old environment. Some Azure CLI devTest environments also require periodic cleanup to
prevent charges for temporary Azure resources that might persist longer than necessary.

In this Azure CLI sample, you learn the following items:

> [!div class="checklist"]
>
> - Delete multiple Azure resources from a script
> - Log script progress to a local TXT file

This sample script was tested in [Azure Cloud Shell][02] using a Bash environment. This script was
also tested successfully in Ubuntu 22.04.3 LTS using [Windows Terminal][03].

## Delete Azure resources filtering by name

Use this script to list and delete resource groups that start with a given word.

```azurecli-interactive
# Set your subscription
subscriptionID=00000000-0000-0000-0000-00000000
az account set --subscription $subscriptionID

# Set your log file location
logFileLocation="myLogName.txt"

# Get the name of all resource groups that start with 'msdocs'
az group list --query "[?starts_with(name, 'msdocs') == \`true\`].name" -o table

# Delete resource groups without a confirmation prompt (--yes)
# Do not wait for the operation to finish (--no-wait)
echo "Deleting resource groups">$logFileLocation
for rgList in $(az group list --query "[?starts_with(name, 'msdocs') == \`true\`].name" -o tsv); 
do
    echo "deleting resource group $rgList">>$logFileLocation
    az group delete --name $rgList --yes --no-wait
done

# read your log file with Linux "cat" command
clear
cat $logFileLocation
```

## Delete Azure resources filtering by creation date

Use this script to list and delete storage accounts that were created within a date range.

```azurecli-interactive
# Set your log file location
logFileLocation="myLogName.txt"

# Set your resource group variable
rgName=<msdocs-rg-0000000>

# Get a list of Azure storage accounts that were created in the last 30 days. Return the results as a table.
saDate=$(date +%F -d "-30days")
az storage account list --resource-group $rgName \
                        --query "[?creationTime >='$saDate'].{saName:name, createdTimeStamp:creationTime}" \
                        --output table

# Delete storage accounts without a confirmation prompt (--yes).
# Do not wait for the operation to finish (--no-wait)
echo "Deleting storage accounts">$logFileLocation
for saList in $(az storage account list --resource-group $rgName \
                        --query "[?creationTime >='$saDate'].{saName:name, createdTimeStamp:creationTime}" \
                        --output tsv);
do
    echo "deleting storage account $saList">>$logFileLocation
    az storage account delete --ids $saList --yes --no-wait
done

# read your log file with Linux "cat" command
clear
cat $logFileLocation
```

## Delete all Azure resources of a type

Delete all Virtual Machines in a resource group

```azurecli-interactive
# Set your resource group variable
rgName=<msdocs-rg-0000000>

az group delete -n $rgName --force-deletion-types Microsoft.Compute/virtualMachines
```

## See also

* [Create Azure resources at scale][01]

<!-- link references -->

[01]: ./create-azure-resources-at-scale.md
[02]: /azure/cloud-shell/overview
[03]: /windows/terminal/
