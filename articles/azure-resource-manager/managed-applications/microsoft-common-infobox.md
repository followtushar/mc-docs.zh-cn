---
title: InfoBox UI 元素
description: 介绍 Azure 门户的 Microsoft.Common.InfoBox UI 元素。 用于在部署托管应用程序时添加文本或警告。
author: rockboyfor
ms.topic: conceptual
origin.date: 06/15/2018
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: bd07ef0c426582685bc2d4d5029168214d068e53
ms.sourcegitcommit: 8de025ca11b62e06ba3762b5d15cc577e0c0f15d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2020
ms.locfileid: "76170680"
---
# <a name="microsoftcommoninfobox-ui-element"></a>Microsoft.Common.InfoBox UI 元素

可添加信息框的控件。 该框包含重要文本或警告，可帮助用户了解他们提供的值。 它还可以链接到详细信息的 URI。

## <a name="ui-sample"></a>UI 示例

![Microsoft.Common.InfoBox](./media/managed-application-elements/microsoft.common.infobox.png)

## <a name="schema"></a>架构

```json
{
  "name": "text1",
  "type": "Microsoft.Common.InfoBox",
  "visible": true,
  "options": {
    "icon": "None",
    "text": "Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo.",
    "uri": "https://www.microsoft.com"
  }
}
```

## <a name="sample-output"></a>示例输出

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="remarks"></a>备注

* 对于 `icon`，请使用“无”、“信息”、“警告”或“错误”     。
* `uri` 属性为可选。

## <a name="next-steps"></a>后续步骤

* 有关创建 UI 定义的简介，请参阅 [CreateUiDefinition 入门](create-uidefinition-overview.md)。
* 有关 UI 元素中的公用属性的说明，请参阅 [CreateUiDefinition 元素](create-uidefinition-elements.md)。

<!-- Update_Description: new article about microsoft common infobox -->
<!--NEW.date: 01/20/2020-->