---
title: 在 Azure IoT Central 中使用 Webhook 触发 Azure Functions
description: 创建每次在 Azure IoT 中心中触发规则时运行的函数应用。
author: viv-liu
ms.author: v-yiso
origin.date: 07/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 931958f62fc87ecd4a6c248a8a37ce517dc3f0e9
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883437"
---
# <a name="trigger-azure-functions-using-webhooks-in-azure-iot-central"></a>在 Azure IoT Central 中使用 Webhook 触发 Azure Functions

本主题适用于构建者和管理员  。

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

使用 Azure Functions 对来自 IoT Central 规则的 Webhook 输出运行无服务器代码。 不必预配 VM 或发布 Web 应用即可使用 Azure Functions，而是可以在无服务器的情况下运行此代码。 先使用 Azure Functions 转换 Webhook 有效负载，然后再将它发送到最终目标，如 SQL 数据库或事件网格。

## <a name="prerequisites"></a>先决条件

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="how-to-connect-azure-functions"></a>如何连接 Azure Functions

1. [在 Azure 门户中创建新的函数应用](https://ms.portal.azure.cn/#create/Microsoft.FunctionApp)。

    ![在 Azure 门户中创建新的函数应用](media/howto-trigger-azure-functions/createfunction.png)

2. 展开函数应用，并选择“Functions”旁边的“+”按钮  。 如果此函数是函数应用中的第一个函数，请选择“在门户中”  作为开发环境，然后选择“继续”  。

    ![在函数应用中选择自定义函数](media/howto-trigger-azure-functions/customfunction.png)

3. 选择“Webhook + API”  模板，然后选择“创建”  。 本主题使用基于 .NET 的 Azure 函数。

    ![选择通用 Webhook 触发器](media/howto-trigger-azure-functions/genericwebhooktrigger.png)

4. 在新函数中，选择“</> 获取函数 URL”  ，然后复制并保存值。 此值将用于配置 webhook。

    ![获取函数的 URL](media/howto-trigger-azure-functions/getfunctionurl.png)

4. 在 IoT Central 中，导航到要连接到函数应用的规则。

5. 添加 Webhook 操作。 输入“显示名称”  ，并将之前复制的函数 URL 粘贴到“回调 URL”  中。

    ![向回调 URL 字段中输入函数 URL](media/howto-trigger-azure-functions/configurewebhook.PNG)

6. 保存规则。 现在当触发规则时，Webhook 会调用要运行的函数应用。 在函数应用中，可以选择“监视器”  以查看函数的调用历史记录。 可以使用 App Insights 或经典视图查看历史记录。

    ![监视函数的调用历史记录](media/howto-trigger-azure-functions/monitorfunction.PNG)

有关详细信息，请访问有关[创建由泛型 Webhook 触发的函数](/azure-functions/functions-create-generic-webhook-triggered-function)的 Azure Functions 文章。

## <a name="next-steps"></a>后续步骤
现在，你已了解如何设置和使用 Webhook，建议下一步是探索[在 Microsoft Flow 中生成工作流](howto-add-microsoft-flow.md)。
