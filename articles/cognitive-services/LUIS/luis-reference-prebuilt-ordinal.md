---
title: 序号预生成实体
titleSuffix: Azure
description: 本文包含了语言理解 (LUIS) 中的序号预构建实体的信息。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/19/19
ms.author: v-lingwu
ms.openlocfilehash: 0982c75a50d09b5d598dd13eeee03396722da9d8
ms.sourcegitcommit: bf4c3c25756ae4bf67efbccca3ec9712b346f871
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2019
ms.locfileid: "65555600"
---
# <a name="ordinal-prebuilt-entity-for-a-luis-app"></a>LUIS 应用的序号预生成实体
序号是一个集合内的对象的数字表示形式：`first`、`second`、`third`。 此实体已定型，因此不需要将包含序号的陈述示例添加到应用程序意向中。 [许多语言区域](luis-reference-prebuilt-entities.md)都支持序号实体。 

## <a name="types-of-ordinal"></a>序号类型
序号托管在 [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45) GitHub 存储库中

## <a name="resolution-for-prebuilt-ordinal-entity"></a>预构建序号实体的解析
以下示例展示了 **builtin.ordinal** 实体的解析。

```json
{
  "query": "Order the second option",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.9993253
  },
  "intents": [
    {
      "intent": "OrderFood",
      "score": 0.9993253
    },
    {
      "intent": "None",
      "score": 0.05046708
    }
  ],
  "entities": [
    {
      "entity": "second",
      "type": "builtin.ordinal",
      "startIndex": 10,
      "endIndex": 15,
      "resolution": {
        "value": "2"
      }
    }
  ]
}
```

## <a name="next-steps"></a>后续步骤

了解[百分比](luis-reference-prebuilt-percentage.md)、[电话号码](luis-reference-prebuilt-phonenumber.md)和[温度](luis-reference-prebuilt-temperature.md)实体。 



