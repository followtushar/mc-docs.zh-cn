---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/02/2020
ms.author: v-junlch
ms.openlocfilehash: aab4834eb4439775168e25ab896be04da8c44a3e
ms.sourcegitcommit: 1ac138a9e7dc7834b5c0b62a133ca5ce2ea80054
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2020
ms.locfileid: "78266060"
---
直接在 function.json 文件中定义绑定属性。 根据绑定类型，可能还需要其他属性。 [队列输出配置](../articles/azure-functions/functions-bindings-storage-queue-output.md#configuration)描述 Azure 存储队列绑定所需的字段。 扩展可以轻松地将绑定添加到 function.json 文件。 

要创建绑定，请右键单击（在 macOS 系统上，请按住 Ctrl 并单击）HttpTrigger 文件夹中的 `function.json` 文件，并选择“添加绑定...”  。请按照提示为新绑定定义以下绑定属性：

| Prompt | Value | 说明 |
| -------- | ----- | ----------- |
| **选择绑定方向** | `out` | 该绑定是输出绑定。 |
| **选择具有方向的绑定...** | `Azure Queue Storage` | 该绑定是 Azure 存储队列绑定。 |
| **用于在代码中标识此绑定的名称** | `msg` | 用于标识代码中引用的绑定参数的名称。 |
| **要将消息发送到的队列** | `outqueue` | 绑定要写入到的队列的名称。 如果 *queueName* 不存在，首次使用绑定时，它会创建该属性。 |
| **从 local.setting.json 中选择设置** | `AzureWebJobsStorage` | 包含存储帐户连接字符串的应用程序设置的名称。 `AzureWebJobsStorage` 设置包含连同函数应用一起创建的存储帐户的连接字符串。 |

绑定将添加到 function.json 中的 `bindings` 数组，应如下所示：

```json
{
 "type": "queue",
 "direction": "out",
 "name": "msg",
 "queueName": "outqueue",
 "connection": "AzureWebJobsStorage"
}

```

