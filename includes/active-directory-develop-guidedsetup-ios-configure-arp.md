---
title: include 文件
description: include 文件
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
origin.date: 09/19/2018
ms.date: 12/07/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: ed57800f708202292a37861f9ce0d957e143d689
ms.sourcegitcommit: 5f2849d5751cb634f1cdc04d581c32296e33ef1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "53070264"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>向应用添加应用程序的注册信息

在此步骤中，需要将应用程序 ID 添加到项目:

1. 在 `ViewController.swift` 中，将以“`let kClientID`”开头的行替换为：
    ```swift
    let kClientID = "[Enter the application Id here]"
    ```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
按住 Control 键并单击 <code>Info.plist</code> 打开上下文菜单，然后单击：<code>Open As</code>> <code>Source Code</code>
</li>
<li>
在 <code>dict</code> 根节点下添加以下代码：
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Enter the application Id here]</string>
        </array>
    </dict>
</array>
```

<!-- ms.date: 12/07/2018 -->