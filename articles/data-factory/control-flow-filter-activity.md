---
title: Azure 数据工厂中的 Filter 活动 | Microsoft Docs
description: Filter 活动将筛选输入。
services: data-factory
documentationcenter: ''
author: WenJason
ms.author: v-jay
manager: digimobile
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
origin.date: 05/04/2018
ms.date: 10/14/2019
ms.openlocfilehash: bc5da6c43670292d6ccc84dd771f1dc3311f971e
ms.sourcegitcommit: aea45739ba114a6b069f782074a70e5dded8a490
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72275424"
---
# <a name="filter-activity-in-azure-data-factory"></a>Azure 数据工厂中的 Filter 活动
可以在管道中使用 Filter 活动将筛选器表达式应用到输入数组。 

## <a name="syntax"></a>语法

```json
{
    "name": "MyFilterActivity",
    "type": "filter",
    "typeProperties": {
        "condition": "<condition>",
        "items": "<input array>"
    }
}
```

## <a name="type-properties"></a>Type 属性

属性 | 说明 | 允许的值 | 必须
-------- | ----------- | -------------- | --------
name | `Filter` 活动的名称。 | String | 是
type | 必须设置为  “filter”。 | String | 是
条件 | 要用于筛选输入的条件。 | 表达式 | 是
items | 应该应用筛选器的输入数组。 | 表达式 | 是

## <a name="example"></a>示例

在此示例中，管道包含两个活动：**Filter** 和 **ForEach**。 Filter 活动配置为筛选输入数组中值大于 3 的项。 然后，ForEach 活动会循环访问筛选的值，并将变量 **test** 设置为当前值。

```json
{
    "name": "PipelineName",
    "properties": {
        "activities": [{
                "name": "MyFilterActivity",
                "type": "filter",
                "typeProperties": {
                    "condition": "@greater(item(),3)",
                    "items": "@pipeline().parameters.inputs"
                }
            },
            {
            "name": "MyForEach",
            "type": "ForEach",
            "dependsOn": [
                {
                    "activity": "MyFilterActivity",
                    "dependencyConditions": [
                        "Succeeded"
                    ]
                }
            ],
            "userProperties": [],
            "typeProperties": {
                "items": {
                    "value": "@activity('MyFilterActivity').output.value",
                    "type": "Expression"
                },
                "isSequential": "false",
                "batchCount": 1,
                "activities": [
                    {
                        "name": "Set Variable1",
                        "type": "SetVariable",
                        "dependsOn": [],
                        "userProperties": [],
                        "typeProperties": {
                            "variableName": "test",
                            "value": {
                                "value": "@string(item())",
                                "type": "Expression"
                            }
                        }
                    }
                ]
            }
        }],
        "parameters": {
            "inputs": {
                "type": "Array",
                "defaultValue": [1, 2, 3, 4, 5, 6]
            }
        },
        "variables": {
            "test": {
                "type": "String"
            }
        },
        "annotations": []
    }
}
```

## <a name="next-steps"></a>后续步骤
查看数据工厂支持的其他控制流活动： 

- [If Condition 活动](control-flow-if-condition-activity.md)
- [Execute Pipeline 活动](control-flow-execute-pipeline-activity.md)
- [For Each 活动](control-flow-for-each-activity.md)
- [Get Metadata 活动](control-flow-get-metadata-activity.md)
- [Lookup 活动](control-flow-lookup-activity.md)
- [Web 活动](control-flow-web-activity.md)
- [Until 活动](control-flow-until-activity.md)
