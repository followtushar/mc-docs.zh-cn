---
title: 威胁检测 - Azure SQL 数据库 | Microsoft 文档
description: 威胁检测会检测异常的数据库活动，指出 Azure SQL 数据库有潜在的安全威胁。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto, carlrab
manager: digimobile
origin.date: 10/25/2018
ms.date: 12/31/2018
ms.openlocfilehash: f8550e6168e44b97d8d0636edc93860a22f4a268
ms.sourcegitcommit: e96e0c91b8c3c5737243f986519104041424ddd5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806413"
---
# <a name="azure-sql-database-threat-detection"></a>Azure SQL 数据库威胁检测

针对 [Azure SQL 数据库](sql-database-technical-overview.md)和 [SQL 数据仓库](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)的 Azure SQL 威胁检测可检测异常活动，这些活动指示对数据库的异常和可能有害的访问或利用企图。

威胁检测包含在 [SQL 高级威胁防护](sql-advanced-threat-protection.md) (ATP) 产品中，该产品是高级 SQL 安全功能统一软件包。 可通过中心 SQL ATP 门户访问和管理威胁检测。

> [!NOTE] 
> 本主题适用于 Azure SQL 服务器，同时也适用于在 Azure SQL 服务器中创建的 SQL 数据库和 SQL 数据仓库数据库。 为简单起见，在提到 SQL 数据库和 SQL 数据仓库时，本文统称 SQL 数据库。

## <a name="what-is-threat-detection"></a>什么是威胁检测？

SQL 威胁检测提供新的安全层，在发生异常活动时会提供安全警报，让客户检测潜在威胁并做出响应。 出现可疑数据库活动、潜在漏洞、SQL 注入攻击和异常数据库访问和查询模式时，用户将收到警报。 SQL 威胁检测将警报与 Azure 安全中心集成，其中包含可疑活动的详细信息以及如何调查和缓解威胁的建议操作。 不必是安全专家，也不需要管理先进的安全监视系统，就能使用 SQL 威胁检测轻松解决数据库的潜在威胁。 

为了提供完整的调查体验，建议启用 [SQL 数据库审核](sql-database-auditing.md)，它会将数据库事件写入到 Azure 存储帐户中的审核日志。  

## <a name="azure-sql-database-threat-detection-alerts"></a>Azure SQL 数据库威胁检测警报 
Azure SQL 数据库威胁检测可检测异常活动（指示异常和可能有害的数据库访问或使用企图），并可触发以下警报：
- **存在易受 SQL 注入攻击的漏洞**：当某个应用程序在数据库中生成错误的 SQL 语句时，会触发此警报。 此警报会指示可能存在易受 SQL 注入攻击的漏洞。 生成错误语句的可能原因有两个：
   - 应用程序代码中的缺陷导致构造出错误的 SQL 语句
   - 应用程序代码或存储过程在构造错误的 SQL 语句时无法清理用户输入，使该语句被 SQL 注入攻击利用
- **潜在 SQL 注入**：当 SQL 攻击有效利用已识别到的应用程序漏洞时，会触发此警报。 这意味着，攻击者正在尝试使用有漏洞的应用程序代码或存储过程注入恶意 SQL 语句。
- **来自异常位置的访问**：当 SQL Server 的访问模式发生更改，有人从异常的地理位置登录到 SQL Server 时，会触发此警报。 在某些情况下，警报会检测合法操作（发布新应用程序或开发人员维护）。 在其他情况下，警报会检测恶意操作（以前的员工、外部攻击者）。
- **来自异常 Azure 数据中心的访问**：当 SQL Server 的访问模式发生更改，有人从最近在此服务器上出现过的 Azure 数据中心登录到 SQL Server 时，会触发此警报。 在某些情况下，警报会检测合法操作（在 Azure、Power BI 或 Azure SQL 查询编辑器中发布新应用程序）。 在其他情况下，警报会检测通过 Azure 资源/服务执行的恶意操作（以前的员工、外部攻击者）。
- **来自陌生主体的访问**：当 SQL Server 的访问模式发生更改，有人使用异常的主体（SQL 用户）登录到 SQL Server 时，会触发此警报。 在某些情况下，警报会检测合法操作（发布新应用程序或开发人员维护）。 在其他情况下，警报会检测恶意操作（以前的员工、外部攻击者）。
- **来自可能有害的应用程序的访问**：当使用可能有害的应用程序访问数据库时，会触发此警报。 在某些情况下，警报会检测操作中的渗透测试。 在其他情况下，警报会检测使用常见攻击工具执行的攻击。
- **暴力破解 SQL 凭据**：当有人使用不同的凭据异常登录并失败很多次时，会触发此警报。 在某些情况下，警报会检测操作中的渗透测试。 在其他情况下，警报会检测暴力破解攻击。

## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>在 Azure 门户中为数据库检测威胁检测警报

SQL 数据库威胁检测功能将其警报与 Azure 安全中心集成。 Azure 门户中“数据库和 SQL ATP”边栏选项卡内的“实时 SQL 威胁检测”磁贴会跟踪活动威胁的状态。

单击“高级威胁防护”可获取在数据库或数据仓库中检测到的活动 SQL 威胁的概述。
   
   ![威胁检测警报 2](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="next-steps"></a>后续步骤

* 详细了解[单一数据库中的威胁检测](sql-database-threat-detection.md)。 
* 详细了解 [SQL 高级威胁防护](sql-advanced-threat-protection.md)。 
* 了解有关 [Azure SQL 数据库审核](sql-database-auditing.md)的详细信息
* 有关定价的更多详细信息，请参阅 [SQL 数据库定价页](https://azure.cn/pricing/details/sql-database/)  