---
title: 使用 Azure 资源管理器模板为 Azure 数据资源管理器创建 IoT 中心数据连接
description: 本文介绍如何使用 Azure 资源管理器模板为 Azure 数据资源管理器创建 IoT 中心数据连接。
author: lucygoldbergmicrosoft
ms.author: v-tawe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
origin.date: 11/28/2019
ms.date: 01/17/2020
ms.openlocfilehash: c9299333adeb54904cd61d6865f7e4b7864c9226
ms.sourcegitcommit: 94e1c9621b8f81a7078f1412b3a73281d0a8668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76123143"
---
# <a name="create-an-iot-hub-data-connection-for-azure-data-explorer-by-using-azure-resource-manager-template"></a>使用 Azure 资源管理器模板为 Azure 数据资源管理器创建 IoT 中心数据连接

> [!div class="op_single_selector"]
> * [Portal](ingest-data-iot-hub.md)
> * [C#](data-connection-iot-hub-csharp.md)
> * [Python](data-connection-iot-hub-python.md)
> * [Azure Resource Manager 模板](data-connection-iot-hub-resource-manager.md)

Azure 数据资源管理器是一项快速且高度可缩放的数据探索服务，适用于日志和遥测数据。 Azure 数据资源管理器提供了从事件中心、IoT 中心和写入 blob 容器的 blob 引入数据（数据加载）的功能。 本文使用 Azure 资源管理器模板为 Azure 数据资源管理器创建 IoT 中心数据连接。

## <a name="prerequisites"></a>必备条件

* 如果没有 Azure 订阅，请在开始前创建一个[试用 Azure 帐户](https://www.azure.cn/pricing/1rmb-trial/)。
* 创建[群集和数据库](create-cluster-database-portal.md)
* 创建[表和列映射](ingest-data-iot-hub.md#create-a-target-table-in-azure-data-explorer)
* 创建[配置了共享访问策略的 IoT 中心](ingest-data-iot-hub.md#create-an-iot-hub)。

## <a name="azure-resource-manager-template-for-adding-an-iot-hub-data-connection"></a>用于添加 IoT 中心数据连接的 Azure 资源管理器模板

以下示例显示用于添加 IoT 中心数据连接的 Azure 资源管理器模板。  可以使用此窗体[在 Azure 门户中编辑和部署模板](/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal#edit-and-deploy-the-template)。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "IotHubs_iothubdemo_name": {
            "type": "string",
            "defaultValue": "iothubdemo",
            "metadata": {
                "description": "Specifies the IoT Hub name."
            }
        },
        "iothubpolices_iothubowner_name": {
            "type": "string",
            "defaultValue": "iothubowner",
            "metadata": {
                "description": "Specifies the shared access policy name."
            }
        },
        "consumergroup_default_name": {
            "type": "string",
            "defaultValue": "$Default",
            "metadata": {
                "description": "Specifies the consumer group of the IoT Hub."
            }
        },
        "Clusters_kustocluster_name": {
            "type": "string",
            "defaultValue": "kustocluster",
            "metadata": {
                "description": "Specifies the name of the cluster"
            }
        },
        "databases_kustodb_name": {
            "type": "string",
            "defaultValue": "kustodb",
            "metadata": {
                "description": "Specifies the name of the database"
            }
        },
        "tables_kustotable_name": {
            "type": "string",
            "defaultValue": "kustotable",
            "metadata": {
                "description": "Specifies the name of the table"
            }
        },
        "mapping_kustomapping_name": {
            "type": "string",
            "defaultValue": "kustomapping",
            "metadata": {
                "description": "Specifies the name of the mapping rule"
            }
        },
        "dataformat_type": {
            "type": "string",
            "defaultValue": "csv",
            "metadata": {
                "description": "Specifies the data format"
            }
        },
        "dataconnections_kustodc_name": {
            "type": "string",
            "defaultValue": "kustodc",
            "metadata": {
                "description": "Name of the data connection to create"
            }
        },
        "subscriptionId": {
            "type": "string",
            "defaultValue": "[subscription().subscriptionId]",
            "metadata": {
                "description": "Specifies the subscriptionId of the IoT Hub"
            }
        },
        "resourceGroup": {
            "type": "string",
            "defaultValue": "[resourceGroup().name]",
            "metadata": {
                "description": "Specifies the resourceGroup of the IoT Hub"
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
    "variables": {
    },
    "resources": [{
            "type": "Microsoft.Kusto/Clusters/Databases/DataConnections",
            "apiVersion": "2019-09-07",
            "name": "[concat(parameters('Clusters_kustocluster_name'), '/', parameters('databases_kustodb_name'), '/', parameters('dataconnections_kustodc_name'))]",
            "location": "[parameters('location')]",
            "kind": "IotHub",
            "properties": {
                "iotHubResourceId": "[resourceId(parameters('subscriptionId'), parameters('resourceGroup'), 'Microsoft.Devices/IotHubs', parameters('IotHubs_iothubdemo_name'))]",
                "consumerGroup": "[parameters('consumergroup_default_name')]",
                "sharedAccessPolicyName": "[parameters('iothubpolices_iothubowner_name')]",
                "tableName": "[parameters('tables_kustotable_name')]",
                "mappingRuleName": "[parameters('mapping_kustomapping_name')]",
                "dataFormat": "[parameters('dataformat_type')]"
            }
        }
    ]
}
```

[!INCLUDE [data-explorer-clean-resources](../../includes/data-explorer-clean-resources.md)]
