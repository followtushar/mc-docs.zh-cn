---
title: 数据存储 - LUIS
titleSuffix: Azure Cognitive Services
description: LUIS 将加密的数据存储在与密钥指定的区域对应的 Azure 数据存储中。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
origin.date: 07/29/2019
ms.date: 09/23/2019
ms.author: v-lingwu
ms.openlocfilehash: 628d05734473df1734cb46551b631506bbe2d7b4
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79292730"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>语言理解 (LUIS) 认知服务中的数据存储和删除
LUIS 将加密的数据存储在与密钥指定的区域对应的 Azure 数据存储中。 此数据将存储 30 天。 

## <a name="utterances"></a>陈述

话语可以存储在两个不同的位置。 

* 在**创作过程**期间，创建话语并将其存储在意向中。 成功的 LUIS 应用需要意向中的话语。 应用发布并在终结点接收查询后，终结点请求的查询字符串 `log=false` 将确定是否存储了终结点话语。 如果存储了终结点，它将成为可在门户的“生成”  部分、“审核终结点话语”  部分中找到的主动学习话语的一部分。 
* 当你**审核终结点话语**时，如果向意向添加一条话语，该话语将不再作为要审核的终结点话语的一部分存储。 它将添加到应用的意向中。 

<a name="utterances-in-an-intent"></a>

### <a name="delete-example-utterances-from-an-intent"></a>从意向中删除示例话语

删除用于定型 [LUIS](luis-reference-regions.md) 的示例话语。 如果从 LUIS 应用中删除某个示例陈述，则会将其从 LUIS Web 服务中删除，导致其无法导出。

<a name="utterances-in-review"></a>

### <a name="delete-utterances-in-review-from-active-learning"></a>从主动学习中删除审核中的话语

可以从 LUIS 在“审查终结点话语”页  中建议的用户话语列表中删除话语。 从此列表中删除表述可以防止系统再将其作为建议提出来，但不会将其从日志中删除。

如果不想主动学习话语，可以禁用主动学习。 禁用主动学习也会禁止记录。

### <a name="disable-logging-utterances"></a>禁止记录话语
禁用主动学习会禁止记录。


<a name="accounts"></a>

## <a name="delete-an-account"></a>删除帐户
如果删除某个帐户，则会删除所有应用及其示例表述和日志。 帐户和数据被永久删除之前，数据将保留 60 天。

从“设置”页可以删除帐户  。 在右上角导航栏中选择帐户名可转到“设置”页  。

## <a name="data-inactivity-as-an-expired-subscription"></a>数据处于非活动状态会被视为过期订阅
出于数据保留和删除的原因，Microsoft 有权自行将处于非活动状态的 LUIS 应用视为过期订阅  。 如果应用在过去 90 天内满足以下条件，则将被视为处于非活动状态： 

* 未对应用发出任何调用  。
* 未进行修改。
* 未分配有当前密钥。
* 无任何用户登录应用。
