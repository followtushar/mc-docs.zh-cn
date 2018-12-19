---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 09/06/2018
ms.date: 11/05/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 9a49811e7a680f622a32ff4f5a346d8990c31de8
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52656392"
---
可以使用通过企业解决方案生成的根证书（推荐），也可以生成自签名证书。 创建根证书以后，请将公共证书数据（不是密钥）作为 Base-64 编码的 X.509 .cer 文件导出，然后将公共证书数据上传到 Azure。

* **企业证书：** 如果要使用企业级解决方案，可以使用现有的证书链。 获取要使用的根证书的 .cer 文件。
* 自签名根证书：如果使用的不是企业证书解决方案，则需要创建自签名根证书。 必须遵循下面的 P2S 证书文章之一中的步骤。 否则，创建的证书将不兼容 P2S 连接，客户端在尝试连接时会收到连接错误。 可以使用 Azure PowerShell、MakeCert 或 OpenSSL。 所提供文章中的步骤可生成兼容证书：

  * [Windows 10 PowerShell 指令](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md)：这些指令需要 Windows 10 和 PowerShell 才能生成证书。 从根证书生成的客户端证书可以安装在任何受支持的 P2S 客户端上。
  * [MakeCert 指令](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md)：如果无权访问用于生成证书的 Windows 10 计算机，则可使用 MakeCert。 MakeCert 已弃用，但仍可使用 MakeCert 生成证书。 从根证书生成的客户端证书可以安装在任何受支持的 P2S 客户端上。
  * [Linux 说明](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)