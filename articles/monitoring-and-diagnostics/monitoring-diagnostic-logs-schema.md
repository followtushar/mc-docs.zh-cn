---
title: Azure 诊断日志支持的服务和架构
description: 了解 Azure 诊断日志支持的服务和事件架构。
author: lingliw
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
origin.date: 10/11/2018
ms.date: 11/26/2018
ms.author: v-lingwu
ms.component: logs
ms.openlocfilehash: 680d75f5e4362f0144c37f8e5cb408311842b323
ms.sourcegitcommit: 579d4e19c2069ba5c7d5cb7e9b233744cc90d1f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "53219575"
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Azure 诊断日志支持的服务、架构和类别

[Azure Monitor 诊断日志](monitoring-overview-of-diagnostic-logs.md)是 Azure 服务发出的日志，用于描述这些服务或资源的操作。 通过 Azure Monitor 提供的所有诊断日志共享公共顶级架构，且每个服务都能灵活地为其事件发出唯一属性。

资源类型（为 `resourceId` 属性时可用）和 `category` 的组合唯一标识架构。 本文介绍了诊断日志的顶级架构以及每个服务的架构链接。

## <a name="top-level-diagnostic-logs-schema"></a>顶级诊断日志架构

| Name | 必需/可选 | 说明 |
|---|---|---|
| time | 必须 | 事件时间戳 (UTC)。 |
| ResourceId | 必须 | 发出事件的资源的资源 ID。 对于租户服务，其形式为 /tenants/tenant-id/providers/provider-name。 |
| tenantId | 对于租户日志是必需的 | 此事件关联到的 Active Directory 租户的租户 ID。 此属性仅用于租户级日志，它不会出现在资源级日志中。 |
| operationName | 必须 | 此事件表示的操作的名称。 如果该事件表示 RBAC 操作，则这是 RBAC 操作名称 （例如 Microsoft.Storage/storageAccounts/blobServices/blobs/Read）。 通常以资源管理器操作的形式建模，即使它们不是实际记录的资源管理器操作 (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | 可选 | 如果使用 API 执行 operationName，则 api-version 与该操作关联（例如 http://myservice.windowsazure.net/object?api-version=2016-06-01)。 如果没有与此操作相对应的 API，则该版本表示该操作的版本，以防与操作相关联的属性在将来发生更改。 |
| category | 必须 | 事件的日志类别。 类别是可以在特定资源上启用或禁用日志的粒度。 在事件的属性 blob 内显示的属性在特定日志类别和资源类型中相同。 典型的日志类别是“Audit”、“Operational”、“Execution”和“Request”。 |
| resultType | 可选 | 事件的状态。 典型值包括“Started”、“In Progress”、“Succeeded”、“Failed”、“Active”和“Resolved”。 |
| resultSignature | 可选 | 事件的子状态。 如果此操作对应于 REST API 调用，则这是相应 REST 调用的 HTTP 状态代码。 |
| resultDescription | 可选 | 此操作的静态文本说明，例如 “获取存储文件”。 |
| durationMs | 可选 | 操作持续时间，以毫秒为单位。 |
| callerIpAddress | 可选 | 调用方 IP 地址，如果该操作对应于来自具有公开 IP 地址的实体的 API 调用。 |
| correlationId | 可选 | 用于将一组相关事件组合在一起的 GUID。 通常情况下，如果两个事件具有相同 operationName，但具有两个不同状态（例如 “Started”和“Succeeded”），则它们共享相同的关联 ID。 这也可以代表事件之间的其他关系。 |
| identity | 可选 | 描述执行操作的用户或应用程序的标识的 JSON Blob。 通常，这将包括 Active Directory 中的授权和声明/JWT 令牌。 |
| 级别 | 可选 | 事件的严重级别。 必须是信息性、警告、错误或严重。 |
| location | 可选 | 发出事件的资源区域，例如 “中国东部”或“中国北部”。 |
| properties | 可选 | 与此特定类别的事件相关的任何扩展属性。 所有自定义/唯一属性都必须放入此架构的“B 部分”。 |

## <a name="service-specific-schemas-for-resource-diagnostic-logs"></a>资源诊断日志的服务特定架构
资源诊断日志的架构因资源和日志类别而异。 此列表显示使诊断日志和链接对服务和类别特定的架构可用的所有服务（如果可用）。

| 服务 | 架构和文档 |
| --- | --- |
| Azure Active Directory | [概述](../active-directory/index.md)、[审核日志架构](../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API 管理 | [API 管理诊断日志](../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| 应用程序网关 |[应用程序网关的诊断日志记录](../application-gateway/application-gateway-diagnostics.md) |
| Azure Batch |[Azure Batch 诊断日志记录](../batch/batch-diagnostics.md) |
| Azure Database for MySQL | [Azure Database for MySQL 诊断日志](../mysql/concepts-server-logs.md#diagnostic-logs) |
| Azure Database for PostgreSQL | [Azure Database for PostgreSQL 诊断日志](../postgresql/concepts-server-logs.md#diagnostic-logs) |
| 认知服务 | 架构不可用。 |
| 适用于 PostgreSQL 的 DB |  架构不可用。 |
| Express Route | 架构不可用。 |
| IoT 中心 | [IoT 中心操作](../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| PowerBI 专用 | 架构不可用。 |
| 恢复服务 | [Azure 备份的数据模型](../backup/backup-azure-reports-data-model.md)|
| 服务总线 |[Azure 服务总线诊断日志](../service-bus-messaging/service-bus-diagnostic-logs.md) |
| 流量管理器 | 架构不可用。 |
| 虚拟网络 | 架构不可用。 |
| 虚拟网络网关 | 架构不可用。 |

## <a name="supported-log-categories-per-resource-type"></a>每种资源类型支持的日志类别
|资源类型|类别|类别显示名称|
|---|---|---|
|Microsoft.AnalysisServices/servers|引擎|引擎|
|Microsoft.AnalysisServices/servers|服务|服务|
|Microsoft.ApiManagement/service|GatewayLogs|与 ApiManagement 网关相关的日志|
|Microsoft.Automation/automationAccounts|JobLogs|作业日志|
|Microsoft.Automation/automationAccounts|JobStreams|作业流|
|Microsoft.Automation/automationAccounts|DscNodeStatus|Dsc 节点状态|
|Microsoft.Batch/batchAccounts|ServiceLog|服务日志|
|Microsoft.ClassicNetwork/networksecuritygroups|网络安全组规则流事件|网络安全组规则流事件|
|Microsoft.CognitiveServices/accounts|审核|审核|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DBforMySQL/servers|MySqlSlowLogs|MySQL 慢查询日志|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLLogs|PostgreSQL 服务器日志|
|Microsoft.Devices/IotHubs|连接|连接|
|Microsoft.Devices/IotHubs|DeviceTelemetry|设备遥测|
|Microsoft.Devices/IotHubs|C2DCommands|C2D 命令|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|设备标识操作|
|Microsoft.Devices/IotHubs|FileUploadOperations|文件上传操作|
|Microsoft.Devices/IotHubs|路由|路由|
|Microsoft.Devices/IotHubs|D2C 孪生操作|D2C 孪生操作|
|Microsoft.Devices/IotHubs|C2DTwinOperations|C2D 孪生操作|
|Microsoft.Devices/IotHubs|TwinQueries|孪生查询|
|Microsoft.Devices/IotHubs|JobsOperations|作业操作|
|Microsoft.Devices/IotHubs|DirectMethods|直接方法|
|Microsoft.Devices/IotHubs|E2EDiagnostics|E2E 诊断（预览版）|
|Microsoft.Devices/IotHubs|配置|配置|
|Microsoft.Devices/provisioningServices|DeviceOperations|设备操作|
|Microsoft.Devices/provisioningServices|ServiceOperations|服务操作|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.DocumentDB/databaseAccounts|QueryRuntimeStatistics|QueryRuntimeStatistics|
|Microsoft.EventHub/namespaces|ArchiveLogs|存档日志|
|Microsoft.EventHub/namespaces|OperationalLogs|运行日志|
|Microsoft.EventHub/namespaces|AutoScaleLogs|自动缩放日志|
|Microsoft.KeyVault/vaults|AuditEvent|审核日志|
|Microsoft.Logic/workflows|WorkflowRuntime|工作流运行时诊断事件|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|集成帐户跟踪事件|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|网络安全组事件|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|网络安全组规则计数器|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|负载均衡器警报事件|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|负载均衡器探测运行状况|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|DDoS 保护通知|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|VM 保护警报|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|应用程序网关访问日志|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|应用程序网关性能日志|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|应用程序网关防火墙日志|
|Microsoft.Network/securegateways|AzureFirewallApplicationRule|Azure 防火墙应用程序规则|
|Microsoft.Network/securegateways|AzureFirewallNetworkRule|Azure 防火墙网络规则|
|Microsoft.Network/azurefirewalls|AzureFirewallApplicationRule|Azure 防火墙应用程序规则|
|Microsoft.Network/azurefirewalls|AzureFirewallNetworkRule|Azure 防火墙网络规则|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|网关诊断日志|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|隧道诊断日志|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|路由诊断日志|
|Microsoft.Network/virtualNetworkGateways|IKEDiagnosticLog|IKE 诊断日志|
|Microsoft.Network/virtualNetworkGateways|P2SDiagnosticLog|P2S 诊断日志|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|流量管理器探测运行状况结果事件|
|Microsoft.Network/expressRouteCircuits|PeeringRouteLog|对等互连路由表日志|
|Microsoft.PowerBIDedicated/capacities|引擎|引擎|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure 备份报告数据|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery 作业|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery 事件|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Recovery 复制项|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Azure Site Recovery 复制统计信息|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery 恢复点|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Azure Site Recovery 复制数据上传速度|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Azure Site Recovery 受保护的磁盘数据改动|
|Microsoft.Search/searchServices|OperationLogs|操作日志|
|Microsoft.ServiceBus/namespaces|OperationalLogs|操作日志|
|Microsoft.Sql/servers/databases|SQLInsights|SQL Insights|
|Microsoft.Sql/servers/databases|AutomaticTuning|自动优化|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|查询存储运行时统计信息|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|查询存储等待统计信息|
|Microsoft.Sql/servers/databases|错误|错误|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|数据库等待统计信息|
|Microsoft.Sql/servers/databases|超时|超时|
|Microsoft.Sql/servers/databases|块|块|
|Microsoft.Sql/servers/databases|死锁数|死锁数|
|Microsoft.Sql/servers/databases|审核|审核日志|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|SQL 安全审核事件|
|Microsoft.Sql/servers/databases|SqlDw_Requests|SQL DW 请求|
|Microsoft.Sql/servers/databases|SqlDw_RequestSteps|SQL DW 请求步骤|
|Microsoft.StreamAnalytics/streamingjobs|执行|执行|
|Microsoft.StreamAnalytics/streamingjobs|创作|创作|

## <a name="next-steps"></a>后续步骤

* [详细了解诊断日志](monitoring-overview-of-diagnostic-logs.md)
* [将资源诊断日志流式传输到事件中心](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [使用 Azure Monitor REST API 更改资源诊断设置](https://msdn.microsoft.com/library/azure/dn931931.aspx)