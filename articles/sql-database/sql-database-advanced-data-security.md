---
title: 高级数据安全 - Azure SQL 数据库 | Microsoft Docs
description: 了解有关发现敏感数据并分类、管理数据库漏洞以及检测可能对 Azure SQL 数据库造成威胁的异常活动的功能。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto
manager: digimobile
origin.date: 02/04/2019
ms.date: 02/25/2019
ms.openlocfilehash: 89e630b619227d7beb49f43afbec8b3c6584aff8
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663835"
---
# <a name="advanced-data-security-for-azure-sql-database"></a>Azure SQL 数据库的高级数据安全

高级数据安全是高级 SQL 安全功能的统一程序包。 它包括用于发现和分类敏感数据、呈现和减少潜在数据库漏洞，以及检测可能表明数据库有威胁的异常活动的功能。 它提供用于启用和管理这些功能的一个转到位置。

## <a name="overview"></a>概述

高级数据安全 (ADS) 提供一组高级 SQL 安全功能，包括数据发现和分类、漏洞评估和威胁检测。

- [数据发现和分类](sql-database-data-discovery-and-classification.md)（当前为预览版）提供内置于 Azure SQL 数据库的功能，可用于发现、分类、标记和保护数据库中的敏感数据。 它可用于直观查看数据库分类状态，以及跟踪对数据库内和其边界外的敏感数据的访问。
- [漏洞评估](sql-vulnerability-assessment.md)是一项易于配置的服务，可以发现、跟踪并帮助修正潜在的数据库漏洞。 它可直观查看安全状态，包括解决安全问题的可操作步骤，并可加强数据库的防御工事。
- [威胁检测](sql-database-threat-detection-overview.md)会检测异常活动，此类活动表明数据库遭到了异常的访问或利用尝试并且可能会造成损害。 它不断监视数据库的可疑活动，并针对潜在漏洞、SQL 注入攻击和异常数据库访问模式提供即时的安全警报。 威胁检测警报提供可疑活动的详细信息，以及如何调查和缓解威胁的推荐操作。

一旦启用 SQL ADS，其包含的所有功能都会启用。 只需单击一次，即可为 SQL 数据库服务器上的所有数据库启用 ADS。 需要属于 [SQL 安全管理器](https://docs.azure.cn/role-based-access-control/built-in-roles#sql-security-manager) 角色、SQL 数据库管理员角色或 SQL Server 管理员角色才能启用或管理 ADS 设置。 

## <a name="getting-started-with-ads"></a>ADS 入门

以下步骤帮助你开始使用 ADS。

## <a name="1-enable-ads"></a>1.启用 ADS

若要启用 ADS，请导航到“安全”下的“高级数据安全”来访问 SQL 数据库服务器。 若要为数据库服务器上的所有数据库启用 ADS，请单击“在服务器上启用高级数据安全”。

![启用 ADS](./media/sql-advanced-protection/enable_atp.png) 

## <a name="2-configure-vulnerability-assessment"></a>2.配置漏洞评估

要开始使用漏洞评估，需要配置保存扫描结果的存储帐户。 为此，请单击“漏洞评估”卡。

![配置 VA](./media/sql-advanced-protection/configure_va.png) 

选择或创建用于保存扫描结果的存储帐户。 还可以打开定期扫描来配置漏洞评估，以每周运行一次自动扫描。 扫描结果摘要会发送到你提供的电子邮件地址。

![VA 设置](./media/sql-advanced-protection/va_settings.png) 

## <a name="3-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>3.开始对数据分类、跟踪漏洞和调查威胁警报

单击“数据发现和分类”卡，查看建议进行分类的敏感列，并使用永久性敏感标签对数据分类。 单击“漏洞评估”卡，查看和管理漏洞扫描和报告，并跟踪安全状况。 如果收到安全警报，请单击“威胁检测”卡，查看警报的详细信息。

## <a name="4-manage-ads-settings-on-your-sql-database-server"></a>4.管理 SQL 数据库服务器上的 ADS 设置

若要查看和管理 ADS 设置，请导航到“安全”下的“高级数据安全”来访问 SQL 数据库服务器。 在此页上，可以启用或禁用 ADS，并修改整个 SQL 数据库服务器的威胁检测设置。

![服务器设置](./media/sql-advanced-protection/server_settings.png) 

## <a name="5-manage-ads-settings-for-a-sql-database"></a>5.管理 SQL 数据库的 ADS 设置

要重写特定数据库的 ADS 设置，请勾选“在数据库级别启用高级数据安全”复选框。 仅当有接收单个数据的单独威胁检测警报这一特殊要求时才使用此选项，以代替接收数据库服务器上所有数据库的警报或在此基础上额外接收单独的威胁检测警报。 

一旦选择此复选框，请单击“此数据库的威胁检测设置”，然后配置此数据库的相关设置。

![数据库和威胁检测设置](./media/sql-advanced-protection/database_threat_detection_settings.png) 

还可以从 ADS 数据库窗格访问数据库服务器的高级数据安全设置。 单击主 ADS 窗格中的“设置”，然后单击“查看高级数据安全服务器设置”。 

![数据库设置](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>后续步骤 

- 详细了解[数据发现和分类](sql-database-data-discovery-and-classification.md) 
- 详细了解[漏洞评估](sql-vulnerability-assessment.md) 
- 详细了解[威胁检测](sql-database-threat-detection.md)