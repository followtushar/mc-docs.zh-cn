---
title: 使用 PowerShell 创建 Azure API 管理实例
description: 按照本教程中的步骤创建新的 Azure API 管理实例。
services: api-management
documentationcenter: ''
author: vladvino
manager: cflower
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: quickstart
ms.custom: mvc
origin.date: 11/15/2017
ms.date: 07/15/2019
ms.author: v-yiso
ms.openlocfilehash: e022b0a1cd43a98f64aeda0cca9f6f7d741a6377
ms.sourcegitcommit: 2b4507745b98b45f1ce3f3d30f397521148ef35a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2020
ms.locfileid: "78213718"
---
# <a name="create-a-new-azure-api-management-service-instance"></a>创建新的 Azure API 管理服务实例

Azure API 管理 (APIM) 可帮助组织将 API 发布给外部、合作伙伴和内部开发人员，以充分发挥其数据和服务的潜力。 API 管理通过开发人员参与、商业洞察力、分析、安全性和保护提供了核心竞争力以确保成功的 API 程序。 使用 APIM 可以为在任何位置托管的现有后端服务创建和管理新式 API 网关。 有关详细信息，请参阅[概述](api-management-key-concepts.md)主题。

本快速入门介绍使用 PowerShell 脚本创建新的 API 管理实例的步骤。 本快速入门演示如何使用可从 Azure 门户运行的 **Azure Cloud Shell**。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
## <a name="log-in-to-azure"></a>登录 Azure

在 https://portal.azure.cn 登录 Azure 门户。


如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块 1.0 或更高版本。 运行 `Get-Module -ListAvailable Az` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com//powershell/azure/install-Az-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则还需运行 `Connect-AzAccount` 来创建与 Azure 的连接。


## <a name="create-resource-group"></a>创建资源组

使用 [New-AzResourceGroup](https://docs.microsoft.com//powershell/module/az.resources/new-azresourcegroup) 创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。 

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location ChinaEast
```

## <a name="create-an-api-management-service"></a>创建 API 管理服务

这是一个长时间运行的操作，可能需要长达 15 分钟。

```azurepowershell-interactive
New-AzApiManagement -ResourceGroupName "myResourceGroup" -Location "China East" -Name "apim-name" -Organization "myOrganization" -AdminEmail "myEmail" -Sku "Developer"
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组和所有相关资源，可以使用 [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) 命令将其删除。

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [导入和发布第一个 API](import-and-publish.md)
