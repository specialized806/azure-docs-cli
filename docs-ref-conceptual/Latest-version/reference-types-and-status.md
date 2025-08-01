---
title: Overview - Azure CLI terminology and support levels
description: Learn about the Azure CLI reference types, statuses and support levels
ms.service: azure-cli
ms.custom: devx-track-azurecli
keywords: azure cli references, reference types, reference status
---

# Overview: Azure CLI terminology and support levels

This article explains Azure CLI terminologies. There are syntax components, reference types, and
statuses.

## Azure CLI syntax components

The Azure CLI syntax is a combination of groups, references, commands, and parameters. Often, the
complete reference command is referred to as a command.

| Azure Service |  Reference group   | Reference subgroup  | Command |            Full reference command             |             Parameter Examples             |
| ------------- | ------------------ | ------------------- | ------- | --------------------------------------------- | ------------------------------------------ |
| Azure CLI     | [az config][01]    |                     |         | `az config`                                   | `--local`, `--output`, `-o`                |
| Azure Network | [az network][02]   | application-gateway | create  | [`az network application-gateway create`][09] | `--name`, `--resource-group`, `--capacity` |
| Azure DevOps  | [az pipelines][03] | agent               | list    | [`az pipelines agent list`][04]               | `--pool-id`, `--agent-name`, `--demands`   |

A reference subgroup can have multiple levels, such as
`az network application-gateway private-link ip-config add`.

| Reference Group |     Subgroup 1      |  Subgroup 2  | Subgroup 3 | Command |
| --------------- | ------------------- | ------------ | ---------- | ------- |
| network         | application-gateway | private-link | ip-config  | add     |

See [Reference list A to Z][05] for a complete list of reference commands.

## What is reference type?

Azure CLI commands are either part of the core Azure CLI service or they're an extension. Extensions
are optional add-ons. The reference type determines the release schedule, status, and installation
method as described here:

|      Term      |                          Core                          |                       Extension                        |
| -------------- | ------------------------------------------------------ | ------------------------------------------------------ |
| **References** | Are part of the primary Azure CLI service              | Are optional reference commands that must be installed |
| **Install**    | Jointly with the [MSI installer][13]                   | Individually with [`az extension add`][07]             |
| **Released**   | On a schedule                                          | As new features or updates become available            |
| **Status**     | Can be GA (Generally Available), preview or deprecated | Also can be GA, preview or deprecated                  |

To get a list of command groups, run `az`.

```azurecli-interactive
# Get list of all command groups
az
```

For a list of extensions, use [`az extension list-available --output table`][08] commands.

```azurecli-interactive
# Get list of extensions
az extension list-available --output table
```

### Core

Azure CLI references that are published as a permanent part of the Azure CLI are referred to as core
references. All core references install with the Azure CLI, and you can't choose a subset of
references. If you run the Azure CLI through Azure Cloud Shell, core references are always up to
date.

### Extension

Extensions aren't shipped as part of the Azure CLI but run as Azure CLI commands. Some extensions
are a permanent part of the Azure CLI, but often, an extension gives you access to preview and
experimental commands. A single reference group, such as `az iot hub`, can have both core and
extension commands. Here are two examples:

|      Full reference command       | Is Core | Is Extension |
| --------------------------------- | ------- | ------------ |
| az iot hub list                   | yes     |              |
| az iot hub job list               |         | yes          |

You're prompted to install an extension upon first use. You can also install an extension by running
the [az extension add][07] command.

You can learn more about extension references including installation and updating in
[Use extensions with the Azure CLI][11]. See [Available extensions for the Azure CLI][10] for a
complete list of extension reference commands.

## What is reference status?

Regardless of reference type, Azure CLI references fall into three status categories: **GA**
(Generally Available), **public preview** or **deprecated**. It's the reference command status (not
type) that determines stability.

|               |    GA     |                                             Public preview                                              |    Deprecated    |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------- | ---------------- |
| **Stability** | Permanent | Can change in response to customer feedback. Is subject to the terms of [Microsoft Azure Previews][12]. | Will be removed. |

> [!NOTE]
> Warnings indicating **public preview** or **deprecated** are part of the Azure CLI command output
> and should be expected.

Most commands and parameters for a single reference have a single status, but this isn't always the
case. A GA reference that is being built out to offer more commands can include both GA and preview
reference commands. As new parameters are added to increase functionality, a single command can also
have parameters that fall under different status categories. Here are example references that have
different statuses:

|    Full reference command    |                                    Parameters                                    |   Type    | GA  | Public preview | Deprecated |
| ---------------------------- | -------------------------------------------------------------------------------- | --------- | --- | -------------- | ---------- |
| `az network dns zone list`   | All                                                                              | Core      | yes |                |            |
| `az network dns zone create` | `--name`, `--resource-group`, `--if-none-match`, `--parent-name`                 | Core      | yes |                |            |
|                              | `--existingParameter5`                                                           | Core      |     |                | yes        |
|                              | `--newParameter6`                                                                | Core      |     | yes            |            |
| `az network vhub list`       | All                                                                              | Extension | yes |                |            |
| `az network vhub create`     | `--address-prefix`, `--name`, `--resource-group`, `-vwan`, `--location`, `--sku` | Extension | yes |                |            |
|                              | `--existingParameter7`                                                           | Extension |     | yes            |            |
|                              | `--newParameter8`                                                                | Extension | yes |                |            |
| `az network firewall create` | All                                                                              | Extension |     | yes            |            |

The previous table is only an example and doesn't represent the current reference statuses for
`az network`.

## See also

- [Azure CLI support lifecycle][06]

<!-- link references -->

[01]: ../../docs-ref-autogen/Latest-version/latest/config.yml
[02]: ../../docs-ref-autogen/Latest-version/latest/network.yml
[03]: ../../docs-ref-autogen/Latest-version/latest/pipelines.yml
[04]: ../../docs-ref-autogen/Latest-version/latest/pipelines/agent.yml
[05]: ../../docs-ref-autogen/Latest-version/latest/reference-index.yml
[06]: ./azure-cli-support-lifecycle.md
[07]: /cli/azure/extension#az-extension-add
[08]: /cli/azure/extension#az-extension-list-available
[09]: /cli/azure/network/application-gateway#az-network-application-gateway-create
[10]: azure-cli-extensions-list.md
[11]: azure-cli-extensions-overview.md
[12]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[13]: install-azure-cli-windows.md
