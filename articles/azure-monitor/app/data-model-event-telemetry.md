---
title: Azure Application Insights 遥测数据模型 - 事件遥测 | Azure Docs
description: 适用于事件遥测的 Application Insights 数据模型
ms.topic: conceptual
author: lingliw
origin.date: 04/25/2017
ms.date: 6/4/2019
ms.reviewer: sergkanz
ms.author: v-lingwu
ms.openlocfilehash: b2880e7ad60453f27ed8c1742495a432b673126f
ms.sourcegitcommit: b7fe28ec2de92b5befe61985f76c8d0216f23430
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78850438"
---
# <a name="event-telemetry-application-insights-data-model"></a>事件遥测：Application Insights 数据模型

可以在 [Application Insights](../../azure-monitor/app/app-insights-overview.md) 中创建事件遥测项，表示应用程序中发生的事件。 通常它是用户交互，如按钮单击或订单结账。 它还可以是应用程序生命周期事件，如初始化或配置更新。 

事件在语义上不一定与请求关联。 但是，如果使用得当，事件遥测比请求或跟踪更重要。 事件表示业务遥测，应是积极程度较低的单独[采样](../../azure-monitor/app/api-filtering-sampling.md)的主体。

## <a name="name"></a>名称

事件名称。 若要允许适当的分组和有用的指标，请限制应用程序，使其生成少量单独的事件名称。 例如，不要为事件中每个生成的实例使用单独的名称。

最大长度：512 个字符

## <a name="custom-properties"></a>自定义属性

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>自定义度量值

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>后续步骤

- 有关 Application Insights 的类型和数据模型，请参阅[数据模型](data-model.md)。
- [编写自定义事件遥测](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)
- 查看 Application Insights 支持的[平台](../../azure-monitor/app/platforms.md)。




