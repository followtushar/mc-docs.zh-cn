---
title: 在 Azure 门户预览中为 VM 创建 FQDN | Azure
description: 了解如何在 Azure 门户预览中为基于 Resource Manager 的虚拟机创建完全限定域名 (FQDN)。
services: virtual-machines-linux
documentationCenter: ''
authors: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager

ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
wacn.date: 04/27/2017
ms.author: iainfou
---

# 在 Azure 门户预览中创建完全限定的域名
在 [Azure 门户预览](https://portal.azure.cn)中使用 Resource Manager 部署模型创建虚拟机 (VM) 时，该门户会为虚拟机自动创建一个公共 IP 资源。可以使用此 IP 地址远程访问 VM。默认情况下，该门户不会创建[完全限定域名](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)（简称 FQDN），但在创建 VM 后，创建完全限定域名相当容易。本文演示了创建 DNS 名称或 FQDN 的步骤。

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

你现在可以使用此 DNS 名称远程连接至 VM，例如用于 `ssh ops@mydns.chinanorth.chinacloudapp.cn`。

## 后续步骤
你的 VM 已经有公共 IP 和 DNS 名称，现在可以部署通用应用程序框架或服务，例如 nginx、MongoDB、Docker 等等。

你也可以详细了解[使用 Resource Manager](../azure-resource-manager/resource-group-overview.md)，以获取有关生成 Azure 部署的提示。

<!---HONumber=Mooncake_Quality_Review_1215_2016-->