---
title: 使用 Azure Monitor 查看指标
description: 本文介绍如何使用 Azure 门户图表和 Azure CLI 监视指标。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/08/2019
ms.date: 02/24/2020
ms.author: v-jay
ms.openlocfilehash: f39bae78cd56f04dd933d4971e313c2cfe50b67a
ms.sourcegitcommit: f5bc5bf51a4ba589c94c390716fc5761024ff353
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77494609"
---
# <a name="monitor-media-services-metrics"></a>监视媒体服务指标 

可以通过 [Azure Monitor](../../azure-monitor/overview.md) 监视指标和诊断日志，以便了解应用程序的执行情况。 有关此功能的详细说明以及使用 Azure 媒体服务指标和诊断日志的原因，请参阅[监视媒体服务指标和诊断日志](media-services-metrics-diagnostic-logs.md)。

Azure Monitor 提供多种方式来与指标交互，包括在门户中制作指标图表、通过 REST API 访问指标，或者使用 CLI 查询指标。 本文介绍如何使用 Azure 门户图表和 Azure CLI 监视指标。

## <a name="prerequisites"></a>必备条件

- [创建媒体服务帐户](create-account-cli-how-to.md)
- 参阅[监视媒体服务指标和诊断日志](media-services-metrics-diagnostic-logs.md)

## <a name="view-metrics-in-azure-portal"></a>在 Azure 门户中查看指标

1. 通过 https://portal.azure.cn 登录到 Azure 门户。
1. 导航到你的 Azure 媒体服务帐户，选择“指标”。 
1. 单击“资源”框，选择要监视其指标的资源。  

    右侧会显示“选择资源”窗口，其中提供了可用资源的列表。  在本例中，你会看到 

    * &lt;媒体服务帐户名称&gt;
    * &lt;媒体服务帐户名称&gt;/&lt;流式处理终结点名称&gt;
    * &lt;存储帐户名称&gt;

    选择资源并按“应用”。  有关支持的资源和指标的详细信息，请参阅[监视媒体服务指标](media-services-metrics-diagnostic-logs.md)。
 
    ![指标](media/media-services-metrics/metrics02.png)
    
    > [!NOTE]
    > 若要在你要监视其指标的资源之间切换，请再次单击“资源”框，然后重复上述步骤。 
1. （可选）为图表命名（按顶部的铅笔图标来编辑名称）。
1. 添加要查看的指标

    ![指标](media/media-services-metrics/metrics03.png)
1. 可将图表固定到仪表板。

## <a name="view-metrics-with-azure-cli"></a>使用 Azure CLI 查看指标

若要使用 CLI 获取“传出”指标，请运行以下 `az monitor metrics` CLI 命令：

```cli
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

若要获取其他指标，请将“传出”替换为所需的指标名称。

## <a name="see-also"></a>另请参阅

* [Azure Monitor 指标](../../azure-monitor/platform/data-platform.md)
* [使用 Azure Monitor 创建、查看和管理指标警报](../../azure-monitor/platform/alerts-metric.md)。

## <a name="next-steps"></a>后续步骤

[诊断日志](media-services-diagnostic-logs-howto.md)
