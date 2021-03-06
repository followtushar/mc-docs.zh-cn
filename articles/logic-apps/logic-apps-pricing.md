---
title: 定价和计费 - Azure 逻辑应用 | Microsoft Docs
description: 了解 Azure 逻辑应用的定价和计费原理
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: v-yiso
ms.reviewer: estfan, LADocs
manager: carmonm
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.topic: article
origin.date: 07/19/2019
ms.date: 10/08/2019
ms.openlocfilehash: 0c24ecff1c991979736fa63f65ba9b984d387b44
ms.sourcegitcommit: 332ae4986f49c2e63bd781685dd3e0d49c696456
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340951"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Azure 逻辑应用的定价模型

借助 [Azure 逻辑应用](../logic-apps/logic-apps-overview.md)可以创建和运行可在云中缩放的自动化集成工作流。 本文介绍 Azure 逻辑应用的计费和定价方式。 有关具体的定价信息，请参阅 [Azure 逻辑应用定价](https://www.azure.cn/pricing/details/logic-apps)。

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>消耗量定价模型

对于在公共或“全球”Azure 逻辑应用服务中运行的新逻辑应用，只需根据实际使用的资源付费。 这些逻辑应用使用基于消耗量的计划和定价模型。 在逻辑应用定义中，每个步骤都是操作。 例如，操作包括： 

* 触发器（特殊的操作）。 所有逻辑应用需将一个触发器用作第一个步骤。
* [“内置”或本机操作](../connectors/apis-list.md#built-in)，例如 HTTP、对 Azure Functions 和 API 管理的调用，等等
* 对[托管连接器](../connectors/apis-list.md##managed-connectors)（如 Outlook 365 等）的调用
* 控制流步骤，例如循环、条件语句，等等

详细了解[触发器](#triggers)和[操作](#actions)的计费方式。

<a name="connectors"></a>

## <a name="connectors"></a>连接器

Azure 逻辑应用连接器通过提供[触发器](#triggers)和/或[操作](#actions)，帮助逻辑应用访问云中或本地的应用、服务和系统。 连接器分类为“标准”或“企业”连接器。 有关这些连接器的概述，请参阅[适用于 Azure 逻辑应用的连接器](../connectors/apis-list.md)。 如果没有预生成的连接器可用于要在逻辑应用中使用的 REST API，则可以创建[自定义连接器](https://docs.microsoft.com/connectors/custom-connectors)，这些连接器只是这些 REST API 的包装器。 自定义连接器按标准连接器计费。 以下部分提供有关触发器和操作的计费方式的详细信息。

<a name="triggers"></a>

## <a name="triggers"></a>触发器

触发器是发生特定事件时创建逻辑应用实例的特殊操作。 触发器以不同方式起作用，从而影响逻辑应用的计量方式。 下面是 Azure 逻辑应用中存在的各种触发器：

* **轮询触发器**：此触发器持续检查终结点是否收到满足创建逻辑应用实例和启动工作流的条件的消息。 即使没有创建逻辑应用实例，逻辑应用也会将每个轮询请求计量为执行。 若要指定轮询间隔，请通过逻辑应用程序设计器设置触发器。

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Webhook 触发器**：此触发器等待客户端向特定的终结点发送请求。 发送到 webhook 终结点的每个请求都会计为操作执行。 例如，请求和 HTTP Webhook 触发器都是 Webhook 触发器。

* 重复周期触发器：  此触发器基于在触发器中设置的重复间隔创建逻辑应用实例。 例如，可以设置每隔三天运行的，或者根据更复杂的计划运行的定期触发器。

<a name="actions"></a>
## <a name="actions"></a>操作

Azure 逻辑应用将 HTTP 等“内置”操作作为本机操作进行计量。 例如，内置操作包括 HTTP 调用、来自 Azure Functions 或 API 管理的调用，以及条件、循环和开关语句等控制流步骤。 每个操作具有自身的操作类型。 例如，调用[连接器](/connectors)的操作为“ApiConnection”类型。 这些连接器分类为“标准”或“企业”连接器，根据各自的[定价](https://www.azure.cn/pricing/details/logic-apps)进行计量。 预览版的企业连接器按标准连接器计费。 

Azure 逻辑应用将所有成功和不成功的操作作为执行进行计量。 但是，逻辑应用不会计量以下操作：

* 由于未满足条件而跳过的操作
* 由于逻辑应用在完成之前停止而未运行的操作

对于在循环内部运行的操作，Azure 逻辑应用会对循环中每个周期的每个操作进行计数。 例如，假设有一个处理列表的“每个”循环。 逻辑应用通过将列表项的数量乘以循环中的操作数来计量该循环中的操作，并加上启动循环的操作。 因此，包含 10 个项的列表的计算公式为 (10 * 1) + 1，即 11 个操作执行。

## <a name="disabled-logic-apps"></a>禁用的逻辑应用

禁用的逻辑应用不收费，因为它们在禁用时无法创建新实例。禁用逻辑应用后，任何当前正在运行的实例可能需要一段时间才能完全停止。

<a name="input-output-sizes"></a>

### <a name="view-action-input-and-output-sizes"></a>查看操作输入和输出大小

1. 在 Azure 门户中，查找并打开逻辑应用。

1. 在逻辑应用的菜单中，选择“概述”  。

1. 在右侧窗格的“运行历史记录”  下，选择具有要检查的输入和输出的运行。

1. 在“逻辑应用运行”  下，选择“运行详细信息”  。

1. 在“逻辑应用运行详细信息”  窗格的“操作表”中（其中列出了每个操作的状态和持续时间），选择要查看的操作。

1. 在“逻辑应用操作”  窗格中，找到该操作的输入和输出的大小（分别显示在“输入链接”  和“输出链接”  下）。
## <a name="next-steps"></a>后续步骤

* [详细了解 Azure 逻辑应用](logic-apps-overview.md)
* [创建第一个逻辑应用](quickstart-create-first-logic-app-workflow.md)
