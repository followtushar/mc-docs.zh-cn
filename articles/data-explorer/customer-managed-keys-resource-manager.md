---
title: 使用 Azure 资源管理器模板在 Azure 数据资源管理器中配置客户托管密钥
description: 本文介绍如何使用 Azure 资源管理器模板在 Azure 数据资源管理器中配置客户托管密钥加密。
author: saguiitay
ms.author: v-tawe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
origin.date: 01/06/2020
ms.date: 02/17/2020
ms.openlocfilehash: d1f7b8d416c275a58ead7d24c145a4192362a017
ms.sourcegitcommit: 7c80405a6b48380814b4b414e9f8a5756c007880
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067803"
---
# <a name="configure-customer-managed-keys-using-the-azure-resource-manager-template"></a>使用 Azure 资源管理器模板配置客户托管密钥

> [!div class="op_single_selector"]
> * [C#](customer-managed-keys-csharp.md)
> * [Azure Resource Manager 模板](customer-managed-keys-resource-manager.md)

[!INCLUDE [data-explorer-configure-customer-managed-keys](../../includes/data-explorer-configure-customer-managed-keys.md)]

## <a name="configure-encryption-with-customer-managed-keys"></a>配置使用客户管理的密钥进行加密

在此部分，我们使用 Azure 资源管理器模板配置客户托管密钥。 默认情况下，Azure 数据资源管理器加密使用 Microsoft 托管密钥。 此步骤将 Azure 数据资源管理器群集配置为使用客户托管密钥，并指定要与群集关联的密钥。

可以使用 Azure 门户或 PowerShell 部署 Azure 资源管理器模板。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "clusterName": {
      "type": "string",
      "defaultValue": "[concat('kusto', uniqueString(resourceGroup().id))]",
      "metadata": {
        "description": "Name of the cluster to create"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "name": "[parameters('clusterName')]",
      "type": "Microsoft.Kusto/clusters",
      "sku": {
        "name": "Standard_D13_v2",
        "tier": "Standard",
        "capacity": 2
      },
      "apiVersion": "2019-09-07",
      "location": "[parameters('location')]",
      "properties": {
        "keyVaultProperties": {
          "keyVaultUri": "<keyVaultUri>",
          "keyName": "<keyName>",
          "keyVersion": "<keyVersion"
        }
      }
    }
  ]
}
```

## <a name="update-the-key-version"></a>更新密钥版本

创建密钥的新版本时，需将群集更新为使用新版本。 首先调用 `Get-AzKeyVaultKey` 以获取最新密钥版本。 然后，将群集的密钥保管库属性更新为使用新的密钥版本，如[使用客户托管密钥配置加密](#configure-encryption-with-customer-managed-keys)中所示。

## <a name="next-steps"></a>后续步骤

* [在 Azure 中保护 Azure 数据资源管理器群集](security.md)
* [配置 Azure 数据资源管理器群集的托管标识](managed-identities.md)
* 通过启用静态加密，[保护 Azure 数据资源管理器中的群集 - Azure 门户](manage-cluster-security.md)。
* [使用 C# 配置客户托管密钥](customer-managed-keys-csharp.md)

