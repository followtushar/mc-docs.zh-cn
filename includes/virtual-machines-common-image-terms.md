---
title: include 文件
description: include 文件
services: virtual-machines-windows, virtual-machines-linux
author: rockboyfor
ms.service: multiple
ms.topic: include
origin.date: 10/09/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: e7d7c04f6b74f8fb620063a5ca426c9e7c1be186
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676290"
---
## <a name="terminology"></a>术语

Azure 中的市场映像具有以下属性：

* **发布者**：创建映像的组织。 示例：Canonical、MicrosoftWindowsServer
* **产品/服务**：发布者创建的一组相关映像的名称。 示例：Ubuntu Server、WindowsServer
* **SKU**：产品/服务的实例，例如分发的主要版本。 示例：16.04-LTS、2016-Datacenter
* **版本**：映像 SKU 的版本号。 

若要在以编程方式部署 VM 时标识市场映像，请以参数的形式单独提供这些值。 有些工具接受映像 URN，它将这些值合并，值之间用冒号 (:) 字符隔开：发布者:产品/服务:Sku:版本。 在 URN 中，可将版本号替换为“latest”，这会选择最新的映像版本。 

如果映像发布者提供附加许可条款和购买条款，则必须接受这些条款并启用编程部署。 以编程方式部署 VM 时，还需要提供“购买计划”参数。 请参阅[部署具有市场条款的映像](#deploy-an-image-with-marketplace-terms)。

<!-- Update_Description: update meta properties, wording update -->