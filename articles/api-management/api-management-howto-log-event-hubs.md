---
title: 如何在 Azure API 管理中将事件记录到 Azure 事件中心
description: 了解如何在 Azure API 管理中将事件记录到 Azure 事件中心。
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/29/2018
ms.author: v-yiso
ms.date: 02/24/2020
ms.openlocfilehash: c06b2e3ae60912294630ebf5cbc1c11ceb5773a5
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77428630"
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>如何在 Azure API 管理中将事件记录到 Azure 事件中心
事件中心是一个高度可缩放的引入服务，每秒可以引入数百万的事件，使用户能够处理和分析连接设备和应用程序生成的海量数据。 事件中心充当事件管道的“前门”，将数据收集到事件中心后，可以使用任何实时分析提供程序或批处理/存储适配器来转换和存储这些数据。 事件中心可将事件流的生成与这些事件的使用分离开来，因此，事件使用者可以根据自己的计划访问事件。

本文介绍如何使用 Azure 事件中心记录 API 管理事件。

## <a name="create-an-azure-event-hub"></a>创建 Azure 事件中心

有关如何创建事件中心以及获取将事件发送到事件中心和从事件中心接收事件所需的连接字符串的详细步骤，请参阅[使用 Azure 门户创建事件中心命名空间和事件中心](../event-hubs/event-hubs-create.md)。

## <a name="create-an-api-management-logger"></a>创建 API 管理记录器
现在有了事件中心，下一步是在 API 管理服务中配置[记录器](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger)，以便它可以将事件记录到事件中心。

使用 [API 管理 REST API](https://aka.ms/apimapi) 配置 API 管理记录器。 有关详细的请求示例，请参阅[如何创建记录器](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger/createorupdate)。

## <a name="configure-log-to-eventhubs-policies"></a>配置 log-to-eventhubs 策略
在 API 管理中配置记录器后，可配置 log-to-eventhubs 策略以记录所需事件。 log-to-eventhubs 策略可在入站策略部分或出站策略部分中使用。

1. 浏览到自己的 APIM 实例。
2. 选择“API”选项卡。
3. 选择要将策略添加到的 API。 在此示例中，我们向 **Unlimited** 产品中的 **Echo API** 添加策略。
4. 选择“所有操作”。 
5. 选择屏幕顶部的“设计”选项卡。
6. 在“入站或出站处理”窗口中，单击三角形（铅笔旁边）。
7. 选择“代码编辑器”。 有关详细信息，请参阅[如何设置或编辑策略](set-edit-policies.md)。
8. 将光标放在 `inbound` 或 `outbound` 策略部分中。
9. 在右侧窗口中，选择“高级策略”   > “记录到 EventHub”  。 这会插入 `log-to-eventhub` 策略语句模板。

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```
将 `logger-id` 替换为 URL 中用于 `{new logger name}` 的值以在上一步中创建记录器。

可使用返回字符串作为 `log-to-eventhub` 元素值的任何表达式。 在此示例中，将记录包含日期和时间、服务名称、请求 ID、请求 IP 地址和操作名称的字符串。

单击“保存”  保存更新后的策略配置。 保存后，策略立即处于活动状态，并且事件记录到指定的事件中心。

## <a name="next-steps"></a>后续步骤
* 了解有关 Azure 事件中心的详细信息
  * [Azure 事件中心入门](../event-hubs/event-hubs-c-getstarted-send.md)
  * [使用 EventProcessorHost 接收消息](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [事件中心编程指南](../event-hubs/event-hubs-programming-guide.md)
* 了解有关 API 管理和事件中心集成的详细信息
  * [记录器实体引用](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger)
  * [log-to-eventhub 策略引用](./api-management-advanced-policies.md#log-to-eventhub)
  * [使用 Azure API 管理、事件中心和 Moesif 监视 API](api-management-log-to-eventhub-sample.md)  



[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
