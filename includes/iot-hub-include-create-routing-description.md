---
title: include 文件
description: include 文件
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/05/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 9461fa736561acc1e686b46ba6ce045fa7023974
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389399"
---
<!-- description of message routing used in the Azure CLI, PowerShell, and RM routing articles.-->

根据模拟设备附加到消息的属性将消息路由到不同资源。 未自定义路由的消息将发送到默认终结点（消息/事件）。 在下一教程中，我们会将消息发送到 IoT 中心，看其如何路由到不同的目标。

|Value |结果|
|------|------|
|级别=“storage” |写入到 Azure 存储。|
|级别=“critical” |写入服务总线队列。 逻辑应用从队列检索消息并使用 Office 365 通过电子邮件发送该消息。|
|默认值 |使用 Power BI 显示此数据。|

第一步是设置终结点，以便将数据路由到其中。 第二步是设置使用该终结点的消息路由。 设置路由后，即可在门户中查看终结点和消息路由。