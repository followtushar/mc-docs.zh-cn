---
title: Azure PowerShell 脚本示例 - 备份 Web 应用 | Microsoft Docs
description: Azure PowerShell 脚本示例 - 备份 Web 应用
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: fc755f82-ca3e-4532-b251-690b699324d6
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
origin.date: 10/30/2017
ms.author: v-biyu
ms.date: 01/21/2019
ms.custom: seodec18
ms.openlocfilehash: c64c13765395077ff93f8ae922565a2c89ab9bb7
ms.sourcegitcommit: 90d5f59427ffa599e8ec005ef06e634e5e843d1e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083906"
---
# <a name="back-up-a-web-app-using-powershell"></a>使用 PowerShell 备份 Web 应用

此示例脚本使用其相关资源，在应用服务中创建 Web 应用，然后为其创建一次性备份。 

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/en-us/powershell/azure/overview)中的说明安装 Azure PowerShell，并运行 `Login-AzureRmAccount` 创建与 Azure 的连接。 

## <a name="sample-script"></a>示例脚本

```powershell
$webappname="mywebapp$(Get-Random -Minimum 100000 -Maximum 999999)"
$storagename="$($webappname)storage"
$container="appbackup"
$location="China East"
$backupname="backup1"

# Create a resource group.
New-AzureRmResourceGroup -Name myResourceGroup -Location $location

# Create a storage account.
$storage = New-AzureRmStorageAccount -ResourceGroupName myResourceGroup `
-Name $storagename -SkuName Standard_LRS -Location $location

# Create a storage container.
New-AzureStorageContainer -Name $container -Context $storage.Context

# Generates an SAS token for the storage container, valid for one month.
# NOTE: You can use the same SAS token to make backups in Web Apps until -ExpiryTime
$sasUrl = New-AzureStorageContainerSASToken -Name $container -Permission rwdl `
-Context $storage.Context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

# Create an App Service plan in Standard tier. Standard tier allows one backup per day.
New-AzureRmAppServicePlan -ResourceGroupName myResourceGroup -Name $webappname `
-Location $location -Tier Standard

# Create a web app.
New-AzureRmWebApp -ResourceGroupName myResourceGroup -Name $webappname `
-Location $location -AppServicePlan $webappname

# Create a one-time backup
New-AzureRmWebAppBackup -ResourceGroupName myResourceGroup -Name $webappname `
-StorageAccountUrl $sasUrl -BackupName $backupname

# List statuses of all backups that are complete or currently executing.
Get-AzureRmWebAppBackupList -ResourceGroupName myResourceGroup -Name $webappname
```


## <a name="clean-up-deployment"></a>清理部署 

运行脚本示例后，可以使用以下命令删除资源组、Web 应用以及所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/en-us/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmStorageAccount](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/new-azurermstorageaccount) | 创建存储帐户。 |
| [New-AzureStorageContainer](https://docs.microsoft.com/en-us/powershell/module/azure.storage/new-azurestoragecontainer) | 创建 Azure 存储容器。 |
| [New-AzureStorageContainerSASToken](https://docs.microsoft.com/en-us/powershell/module/azure.storage/new-azurestoragecontainersastoken) | 为 Azure 存储容器生成 SAS 令牌。  |
| [New-AzureRmAppServicePlan](https://docs.microsoft.com/en-us/powershell/module/azurerm.websites/new-azurermappserviceplan) | 创建应用服务计划。 |
| [New-AzureRmWebApp](https://docs.microsoft.com/en-us/powershell/module/azurerm.websites/new-azurermwebapp) | 创建 Web 应用。 |
| [New-AzureRmWebAppBackup](https://docs.microsoft.com/en-us/powershell/module/azurerm.websites/new-azurermwebappbackup) | 创建 Web 应用的备份。 |
| [Get-AzureRmWebAppBackupList](https://docs.microsoft.com/en-us/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | 获取 Web 应用的备份列表。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/en-us/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../samples-powershell.md)中找到 Azure 应用服务 Web 应用的其他 Azure Powershell 示例。