---
title: 资源的标记支持
description: 显示支持标记的 Azure资源类型。 提供所有 Azure 服务的详细信息。
ms.topic: conceptual
origin.date: 11/22/2019
ms.author: v-yeche
ms.date: 01/06/2020
ms.openlocfilehash: 537a3f3dbbc2399e061940f1f6efc59634d074d9
ms.sourcegitcommit: de60969043e6dd8ef706ed13e0684a7c35b26bdb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2020
ms.locfileid: "76550222"
---
# <a name="tag-support-for-azure-resources"></a>Azure 资源的标记支持
本文介绍某一资源类型是否支持[标记](tag-resources.md)。 标记为“支持标记”  的列指示资源类型是否具有标记的属性。 

<!--Not Avaialble on [Cost Management cost analysis](../../cost-management/quick-acm-cost-analysis.md#understanding-grouping-and-filtering-options)-->
<!--Not Avaialble on [Azure billing invoice and daily usage data](../../billing/billing-download-azure-invoice-daily-usage-date.md)-->
<!--Not Available on [tag-support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv)-->

跳转到资源提供程序命名空间：
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Features](#microsoftfeatures)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.Scheduler](#microsoftscheduler)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SQL](#microsoftsql)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Web](#microsoftweb)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | DomainServices | 是 | 是 |
> | DomainServices / oucontainer | 否 | 否 |

<!--Not Available on ## microsoft.aadiam-->
<!--Not Available on ## Microsoft.Addons-->
<!--Not Available on ## Microsoft.ADHybridHealthService-->

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | 配置 | 否 | 否 |
> | generateRecommendations | 否 | 否 |
> | metadata | 否 | 否 |
> | 建议 | 否 | 否 |
> | 禁止显示 | 否 | 否 |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | actionRules | 是 | 是 |
> | alerts | 否 | 否 |
> | alertsList | 否 | 否 |
> | alertsMetaData | 否 | 否 |
> | alertsSummary | 否 | 否 |
> | alertsSummaryList | 否 | 否 |
> | 反馈 | 否 | 否 |
> | smartDetectorAlertRules | 是 | 是 |
> | smartDetectorRuntimeEnvironments | 否 | 否 |
> | smartGroups | 否 | 否 |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | servers | 是 | 是 |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | reportFeedback | 否 | 否 |
> | 服务 | 是 | 是 |
> | validateServiceName | 否 | 否 |

<!--Not Available on ## Microsoft.AppConfiguration-->
<!--Not Available on ## Microsoft.AppPlatform-->
<!--Not Available on ## Microsoft.Attestation-->

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | classicAdministrators | 否 | 否 |
> | dataAliases | 否 | 否 |
> | denyAssignments | 否 | 否 |
> | elevateAccess | 否 | 否 |
> | findOrphanRoleAssignments | 否 | 否 |
> | 锁定 | 否 | 否 |
> | 权限 | 否 | 否 |
> | policyAssignments | 否 | 否 |
> | policyDefinitions | 否 | 否 |
> | policySetDefinitions | 否 | 否 |
> | providerOperations | 否 | 否 |
> | roleAssignments | 否 | 否 |
> | roleDefinitions | 否 | 否 |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | automationAccounts | 是 | 是 |
> | automationAccounts / configurations | 是 | 是 |
> | automationAccounts / jobs | 否 | 否 |
> | automationAccounts / runbooks | 是 | 是 |
> | automationAccounts / softwareUpdateConfigurations | 否 | 否 |
> | automationAccounts / webhooks | 否 | 否 |

<!--Not Avaialble on ## Microsoft.Azconfig-->
<!--Not Avaialble on ## Microsoft.Azure.Geneva-->

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | b2cDirectories | 是 | 否 |
> | b2ctenants | 否 | 否 |

<!--Not Avaialble on ## Microsoft.AzureData-->

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | registrations | 是 | 是 |
> | registrations / customerSubscriptions | 否 | 否 |
> | registrations / products | 否 | 否 |
> | verificationKeys | 否 | 否 |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | batchAccounts | 是 | 是 |

<!--Not Available on ## Microsoft.Billing -->
<!--Not Available on ## Microsoft.BingMaps-->
<!--Not Available on ## Microsoft.BizTalkServices-->
<!--Not Available on ## Microsoft.Blockchain-->
<!--Not Available on ## Microsoft.Blueprint-->
<!--Not Available on ## Microsoft.Blueprint-->
<!--Not Available on ## Microsoft.BotService-->

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | Redis | 是 | 是 |
> | RedisConfigDefinition | 否 | 否 |

<!--Not Available on ## Microsoft.Capacity-->

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | 否 | 否 |
> | CdnWebApplicationFirewallPolicies | 是 | 是 |
> | edgenodes | 否 | 否 |
> | 配置文件 | 是 | 是 |
> | profiles/endpoints | 是 | 是 |
> | profiles / endpoints / customdomains | 否 | 否 |
> | profiles / endpoints / origins | 否 | 否 |
> | validateProbe | 否 | 否 |

<!--Not Available on ## Microsoft.CertificateRegistration-->


## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | capabilities | 否 | 否 |
> | domainNames | 否 | 否 |
> | domainNames / capabilities | 否 | 否 |
> | domainNames / internalLoadBalancers | 否 | 否 |
> | domainNames / serviceCertificates | 否 | 否 |
> | domainNames / slots | 否 | 否 |
> | domainNames / slots / roles | 否 | 否 |
> | domainNames / slots / roles / metricDefinitions | 否 | 否 |
> | domainNames / slots / roles / metrics | 否 | 否 |
> | moveSubscriptionResources | 否 | 否 |
> | operatingSystemFamilies | 否 | 否 |
> | operatingSystems | 否 | 否 |
> | quotas | 否 | 否 |
> | resourceTypes | 否 | 否 |
> | validateSubscriptionMoveAvailability | 否 | 否 |
> | virtualMachines | 否 | 否 |
> | virtualMachines / diagnosticSettings | 否 | 否 |
> | virtualMachines / metricDefinitions | 否 | 否 |
> | virtualMachines / metrics | 否 | 否 |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | classicInfrastructureResources | 否 | 否 |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | capabilities | 否 | 否 |
> | expressRouteCrossConnections | 否 | 否 |
> | expressRouteCrossConnections / peerings | 否 | 否 |
> | gatewaySupportedDevices | 否 | 否 |
> | networkSecurityGroups | 否 | 否 |
> | quotas | 否 | 否 |
> | reservedIps | 否 | 否 |
> | virtualNetworks | 否 | 否 |
> | virtualNetworks / remoteVirtualNetworkPeeringProxies | 否 | 否 |
> | virtualNetworks / virtualNetworkPeerings | 否 | 否 |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | capabilities | 否 | 否 |
> | disks | 否 | 否 |
> | images | 否 | 否 |
> | osImages | 否 | 否 |
> | osPlatformImages | 否 | 否 |
> | publicImages | 否 | 否 |
> | quotas | 否 | 否 |
> | storageAccounts | 否 | 否 |
> | storageAccounts / blobServices | 否 | 否 |
> | storageAccounts / fileServices | 否 | 否 |
> | storageAccounts / metricDefinitions | 否 | 否 |
> | storageAccounts / metrics | 否 | 否 |
> | storageAccounts / queueServices | 否 | 否 |
> | storageAccounts / services | 否 | 否 |
> | storageAccounts / services / diagnosticSettings | 否 | 否 |
> | storageAccounts / services / metricDefinitions | 否 | 否 |
> | storageAccounts / services / metrics | 否 | 否 |
> | storageAccounts / tableServices | 否 | 否 |
> | storageAccounts / vmImages | 否 | 否 |
> | vmImages | 否 | 否 |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | accounts | 是 | 是 |

<!--Not Available on ## Microsoft.Commerce-->
## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | availabilitySets | 是 | 是 |
> | diskEncryptionSets | 是 | 是 |
> | disks | 是 | 是 |
> | galleries | 是 | 是 |
> | galleries / applications | 否 | 否 |
> | galleries / applications / versions | 否 | 否 |
> | galleries/images | 否 | 否 |
> | galleries/images/versions | 否 | 否 |
> | hostGroups | 是 | 是 |
> | hostGroups / hosts | 是 | 是 |
> | images | 是 | 是 |
> | proximityPlacementGroups | 是 | 是 |
> | restorePointCollections | 是 | 是 |
> | restorePointCollections / restorePoints | 否 | 否 |
> | sharedVMImages | 是 | 是 |
> | sharedVMImages / versions | 否 | 否 |
> | snapshots | 是 | 是 |
> | virtualMachines | 是 | 是 |
> | virtualMachines / extensions | 是 | 是 |
> | virtualMachines / metricDefinitions | 否 | 否 |
> | virtualMachineScaleSets | 是 | 是 |
> | virtualMachineScaleSets / extensions | 否 | 否 |
> | virtualMachineScaleSets / networkInterfaces | 否 | 否 |
> | virtualMachineScaleSets / publicIPAddresses | 否 | 否 |
> | virtualMachineScaleSets / virtualMachines | 否 | 否 |
> | virtualMachineScaleSets / virtualMachines / networkInterfaces | 否 | 否 |

<!--Not Available on ## Microsoft.Consumption -->

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | containerGroups | 是 | 是 |
> | serviceAssociationLinks | 否 | 否 |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | registries | 是 | 是 |
> | registries / builds | 否 | 否 |
> | registries / builds / cancel | 否 | 否 |
> | registries / builds / getLogLink | 否 | 否 |
> | registries / buildTasks | 是 | 是 |
> | registries / buildTasks / steps | 否 | 否 |
> | registries / eventGridFilters | 否 | 否 |
> | registries / generateCredentials | 否 | 否 |
> | registries / getBuildSourceUploadUrl | 否 | 否 |
> | registries / GetCredentials | 否 | 否 |
> | registries / importImage | 否 | 否 |
> | registries / queueBuild | 否 | 否 |
> | registries / regenerateCredential | 否 | 否 |
> | registries / regenerateCredentials | 否 | 否 |
> | registries/replications | 是 | 是 |
> | registries / runs | 否 | 否 |
> | registries / runs / cancel | 否 | 否 |
> | registries / scheduleRun | 否 | 否 |
> | registries / scopeMaps | 否 | 否 |
> | registries / taskRuns | 是 | 是 |
> | registries/tasks | 是 | 是 |
> | registries / tokens | 否 | 否 |
> | registries / updatePolicies | 否 | 否 |
> | registries/webhooks | 是 | 是 |
> | registries / webhooks / getCallbackConfig | 否 | 否 |
> | registries / webhooks / ping | 否 | 否 |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | containerServices | 是 | 是 |
> | managedClusters | 是 | 是 |
> | openShiftManagedClusters | 是 | 是 |

<!--Not Available on ## Microsoft.ContentModerator -->
<!--Not Available on ## Microsoft.CortanaAnalytics-->

<!--Not Available on  ## Microsoft.CostManagement-->
<!--Not Available on  ## Microsoft.CustomerInsights-->
<!--Not Available on  ## Microsoft.CustomerLockbox-->
<!--Not Available on  ## Microsoft.CustomProviders-->
<!--Not Available on  ## Microsoft.DataBox-->
<!--Not Available on  ## Microsoft.DataBoxEdge-->
<!--Not Available on  ## Microsoft.Databricks-->
<!--Not Available on  ## Microsoft.DataCatalog-->
<!--Not Available on  ## Microsoft.DataConnect-->

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | dataFactories | 是 | 否 |
> | dataFactories / diagnosticSettings | 否 | 否 |
> | dataFactories / metricDefinitions | 否 | 否 |
> | dataFactorySchema | 否 | 否 |
> | factories | 是 | 否 |
> | factories / integrationRuntimes | 否 | 否 |

<!--Not Available on  ## Microsoft.DataLakeAnalytics-->
<!--Not Available on  ## Microsoft.DataLakeStore-->

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | services | 否 | 否 |
> | services/projects | 否 | 否 |

<!--Not Available on  ## Microsoft.DataShare-->

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | servers | 是 | 是 |
> | servers / advisors | 否 | 否 |
> | servers / privateEndpointConnectionProxies | 否 | 否 |
> | servers / privateEndpointConnections | 否 | 否 |
> | servers / privateLinkResources | 否 | 否 |
> | servers / queryTexts | 否 | 否 |
> | servers / recoverableServers | 否 | 否 |
> | servers / topQueryStatistics | 否 | 否 |
> | servers / virtualNetworkRules | 否 | 否 |
> | servers / waitStatistics | 否 | 否 |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | servers | 是 | 是 |
> | servers / advisors | 否 | 否 |
> | servers / privateEndpointConnectionProxies | 否 | 否 |
> | servers / privateEndpointConnections | 否 | 否 |
> | servers / privateLinkResources | 否 | 否 |
> | servers / queryTexts | 否 | 否 |
> | servers / recoverableServers | 否 | 否 |
> | servers / topQueryStatistics | 否 | 否 |
> | servers / virtualNetworkRules | 否 | 否 |
> | servers / waitStatistics | 否 | 否 |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | serverGroups | 是 | 是 |
> | servers | 是 | 是 |
> | servers / advisors | 否 | 否 |
> | servers / keys | 否 | 否 |
> | servers / privateEndpointConnectionProxies | 否 | 否 |
> | servers / privateEndpointConnections | 否 | 否 |
> | servers / privateLinkResources | 否 | 否 |
> | servers / queryTexts | 否 | 否 |
> | servers / recoverableServers | 否 | 否 |
> | servers / topQueryStatistics | 否 | 否 |
> | servers / virtualNetworkRules | 否 | 否 |
> | servers / waitStatistics | 否 | 否 |
> | serversv2 | 是 | 是 |

<!--Not Available on ## Microsoft.DeploymentManager-->
<!--Not Available on ## Microsoft.DesktopVirtualization-->

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | ElasticPools | 是 | 是 |
> | ElasticPools / IotHubTenants | 是 | 是 |
> | IotHubs | 是 | 是 |
> | IotHubs / eventGridFilters | 否 | 否 |
> | ProvisioningServices | 是 | 是 |
> | usages | 否 | 否 |

<!--Not Available on ## Microsoft.DevOps-->
<!--Not Available on ## Microsoft.DevSpaces-->
<!--Not Available on ## Microsoft.DevTestLab-->

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | databaseAccountNames | 否 | 否 |
> | databaseAccounts | 是 | 是 |

<!--Not Available on ## Microsoft.DomainRegistration-->
<!--Not Available on ## Microsoft.DynamicsLcs-->
<!--Not Available on ## Microsoft.EnterpriseKnowledgeGraph-->

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | domains | 是 | 是 |
> | domains / topics | 否 | 否 |
> | eventSubscriptions | 否 | 否 |
> | extensionTopics | 否 | 否 |
> | topics | 是 | 是 |
> | topicTypes | 否 | 否 |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | clusters | 是 | 是 |
> | namespaces | 是 | 是 |
> | namespaces / authorizationrules | 否 | 否 |
> | namespaces / disasterrecoveryconfigs | 否 | 否 |
> | namespaces / eventhubs | 否 | 否 |
> | namespaces / eventhubs / authorizationrules | 否 | 否 |
> | namespaces / eventhubs / consumergroups | 否 | 否 |
> | namespaces / networkrulesets | 否 | 否 |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | features | 否 | 否 |
> | providers | 否 | 否 |

<!--Not Available on ## Microsoft.Gallery-->

<!--Not Available on ## Microsoft.Genomics-->

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | configurationProfileAssignments | 否 | 否 |
> | guestConfigurationAssignments | 否 | 否 |
> | software | 否 | 否 |
> | softwareUpdateProfile | 否 | 否 |
> | softwareUpdates | 否 | 否 |

<!--Not Available on  ## Microsoft.HanaOnAzure-->
<!--Not Available on  ## Microsoft.HardwareSecurityModules-->

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | clusters | 是 | 是 |
> | clusters/applications | 否 | 否 |

<!--Not Available on  ## Microsoft.HealthcareApis-->
<!--Not Available on  ## Microsoft.HybridCompute-->
<!--Not Available on  ## Microsoft.HybridData-->
<!--Not Available on ## Microsoft.Hydra-->

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | jobs | 是 | 是 |

<!--Not Available on  ## Microsoft.Intune-->

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | appTemplates | 否 | 否 |
> | IoTApps | 是 | 是 |

<!--Not Available on  ## Microsoft.IoTSpaces-->

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | deletedVaults | 否 | 否 |
> | hsmPools | 是 | 是 |
> | vaults | 是 | 是 |
> | vaults / accessPolicies | 否 | 否 |
> | vaults / eventGridFilters | 否 | 否 |
> | vaults / secrets | 否 | 否 |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | clusters | 是 | 是 |
> | clusters / attacheddatabaseconfigurations | 否 | 否 |
> | clusters / databases | 否 | 否 |
> | clusters / databases / dataconnections | 否 | 否 |
> | clusters / databases / eventhubconnections | 否 | 否 |
> | clusters / sharedidentities | 否 | 否 |

<!--Not Available on  ## Microsoft.LabServices-->

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | hostingEnvironments | 是 | 是 |
> | integrationAccounts | 是 | 是 |
> | integrationServiceEnvironments | 是 | 是 |
> | integrationServiceEnvironments / managedApis | 是 | 是 |
> | isolatedEnvironments | 是 | 是 |
> | workflows | 是 | 是 |

<!--Not Available on ## Microsoft.MachineLearning-->
<!--Not Available on ## Microsoft.MachineLearningServices-->

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | 标识 | 否 | 否 |
> | userAssignedIdentities | 是 | 是 |

<!--Not Available on ## Microsoft.ManagedLab-->
<!--Not Available on ## Microsoft.ManagedServices-->

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | getEntities | 否 | 否 |
> | managementGroups | 否 | 否 |
> | resources | 否 | 否 |
> | startTenantBackfill | 否 | 否 |
> | tenantBackfillStatus | 否 | 否 |

<!--Not Available on  ## Microsoft.Maps-->
<!--Not Available on  ## Microsoft.Marketplace-->
<!--Not Available on  ## Microsoft.MarketplaceApps-->
<!--Not Available on  ## Microsoft.MarketplaceOrdering-->

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | mediaservices | 是 | 是 |
> | mediaservices / accountFilters | 否 | 否 |
> | mediaservices / assets | 否 | 否 |
> | mediaservices / assets / assetFilters | 否 | 否 |
> | mediaservices / contentKeyPolicies | 否 | 否 |
> | mediaservices / eventGridFilters | 否 | 否 |
> | mediaservices / liveEventOperations | 否 | 否 |
> | mediaservices / liveEvents | 是 | 是 |
> | mediaservices / liveEvents / liveOutputs | 否 | 否 |
> | mediaservices / liveOutputOperations | 否 | 否 |
> | mediaservices / mediaGraphs | 否 | 否 |
> | mediaservices / streamingEndpointOperations | 否 | 否 |
> | mediaservices / streamingEndpoints | 是 | 是 |
> | mediaservices / streamingLocators | 否 | 否 |
> | mediaservices / streamingPolicies | 否 | 否 |
> | mediaservices / transforms | 否 | 否 |
> | mediaservices / transforms / jobs | 否 | 否 |

<!--Not Available on  ## Microsoft.Microservices4Spring-->
<!--Not Available on  ## Microsoft.Migrate-->
<!--Not Available on  ## Microsoft.MixedReality-->
<!--Not Available on  ## Microsoft.NetApp-->

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | applicationGateways | 是 | 是 |
> | applicationGatewayWebApplicationFirewallPolicies | 是 | 是 |
> | applicationSecurityGroups | 是 | 是 |
> | azureFirewallFqdnTags | 否 | 否 |
> | azureFirewalls | 是 | 否 |
> | bastionHosts | 是 | 是 |
> | bgpServiceCommunities | 否 | 否 |
> | connections | 是 | 是 |
> | ddosCustomPolicies | 是 | 是 |
> | ddosProtectionPlans | 是 | 是 |
> | dnsOperationStatuses | 否 | 否 |
> | dnszones | 是 | 是 |
> | dnszones / A | 否 | 否 |
> | dnszones / AAAA | 否 | 否 |
> | dnszones / all | 否 | 否 |
> | dnszones / CAA | 否 | 否 |
> | dnszones / CNAME | 否 | 否 |
> | dnszones / MX | 否 | 否 |
> | dnszones / NS | 否 | 否 |
> | dnszones / PTR | 否 | 否 |
> | dnszones / recordsets | 否 | 否 |
> | dnszones / SOA | 否 | 否 |
> | dnszones / SRV | 否 | 否 |
> | dnszones / TXT | 否 | 否 |
> | expressRouteCircuits | 是 | 是 |
> | expressRouteCrossConnections | 是 | 是 |
> | expressRouteGateways | 是 | 是 |
> | expressRoutePorts | 是 | 是 |
> | expressRouteServiceProviders | 否 | 否 |
> | firewallPolicies | 是 | 是 |
> | frontdoors | 是，但受限制（请参见[下面的注释](#frontdoor)） | 是 |
> | frontdoorWebApplicationFirewallManagedRuleSets | 是，但受限制（请参见[下面的注释](#frontdoor)） | 否 |
> | frontdoorWebApplicationFirewallPolicies | 是，但受限制（请参见[下面的注释](#frontdoor)） | 是 |
> | getDnsResourceReference | 否 | 否 |
> | internalNotify | 否 | 否 |
> | loadBalancers | 是 | 否 |
> | localNetworkGateways | 是 | 是 |
> | natGateways | 是 | 是 |
> | networkIntentPolicies | 是 | 是 |
> | networkInterfaces | 是 | 是 |
> | networkProfiles | 是 | 是 |
> | networkSecurityGroups | 是 | 是 |
> | networkWatchers | 是 | 否 |
> | networkWatchers / connectionMonitors | 是 | 否 |
> | networkWatchers / lenses | 是 | 否 |
> | networkWatchers / pingMeshes | 是 | 否 |
> | p2sVpnGateways | 是 | 是 |
> | privateDnsOperationStatuses | 否 | 否 |
> | privateDnsZones | 是 | 是 |
> | privateDnsZones / A | 否 | 否 |
> | privateDnsZones / AAAA | 否 | 否 |
> | privateDnsZones / all | 否 | 否 |
> | privateDnsZones / CNAME | 否 | 否 |
> | privateDnsZones / MX | 否 | 否 |
> | privateDnsZones / PTR | 否 | 否 |
> | privateDnsZones / SOA | 否 | 否 |
> | privateDnsZones / SRV | 否 | 否 |
> | privateDnsZones / TXT | 否 | 否 |
> | privateDnsZones / virtualNetworkLinks | 是 | 是 |
> | privateEndpoints | 是 | 是 |
> | privateLinkServices | 是 | 是 |
> | publicIPAddresses | 是 | 是 |
> | publicIPPrefixes | 是 | 是 |
> | routeFilters | 是 | 是 |
> | routeTables | 是 | 是 |
> | serviceEndpointPolicies | 是 | 是 |
> | trafficManagerGeographicHierarchies | 否 | 否 |
> | trafficmanagerprofiles | 是 | 是 |
> | trafficmanagerprofiles/heatMaps | 否 | 否 |
> | trafficManagerUserMetricsKeys | 否 | 否 |
> | virtualHubs | 是 | 是 |
> | virtualNetworkGateways | 是 | 是 |
> | virtualNetworks | 是 | 是 |
> | virtualNetworkTaps | 是 | 是 |
> | virtualWans | 是 | 是 |
> | vpnGateways | 是 | 否 |
> | vpnSites | 是 | 是 |
> | webApplicationFirewallPolicies | 是 | 是 |

<a name="frontdoor" />

<!--Not Available on Azure Front Door Service-->

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | namespaces | 是 | 否 |
> | namespaces / notificationHubs | 是 | 否 |

<!--Not Available on ## Microsoft.ObjectStore-->
<!--Not Available on ## Microsoft.OffAzure-->

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | clusters | 是 | 是 |
> | devices | 否 | 否 |
> | linkTargets | 否 | 否 |
> | storageInsightConfigs | 否 | 否 |
> | workspaces | 是 | 是 |
> | workspaces / dataSources | 否 | 否 |
> | workspaces / linkedServices | 否 | 否 |
> | workspaces / query | 否 | 否 |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | managementassociations | 否 | 否 |
> | managementconfigurations | 是 | 是 |
> | solutions | 是 | 是 |
> | 视图 | 是 | 是 |

<!--Not Available on ## Microsoft.Peering-->

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | policyEvents | 否 | 否 |
> | policyMetadata | 否 | 否 |
> | policyStates | 否 | 否 |
> | policyTrackedResources | 否 | 否 |
> | remediations | 否 | 否 |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | consoles | 否 | 否 |
> | dashboards | 是 | 是 |
> | userSettings | 否 | 否 |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | workspaceCollections | 是 | 是 |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | capacities | 是 | 是 |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | backupProtectedItems | 否 | 否 |
> | vaults | 是 | 是 |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | namespaces | 是 | 是 |
> | namespaces / authorizationrules | 否 | 否 |
> | namespaces / hybridconnections | 否 | 否 |
> | namespaces / hybridconnections / authorizationrules | 否 | 否 |
> | namespaces / wcfrelays | 否 | 否 |
> | namespaces / wcfrelays / authorizationrules | 否 | 否 |

<!--Not Available on  ## Microsoft.RemoteApp-->

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | 查询 | 是 | 是 |
> | resourceChangeDetails | 否 | 否 |
> | resourceChanges | 否 | 否 |
> | resources | 否 | 否 |
> | resourcesHistory | 否 | 否 |
> | subscriptionsStatus | 否 | 否 |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | availabilityStatuses | 否 | 否 |
> | childAvailabilityStatuses | 否 | 否 |
> | childResources | 否 | 否 |
> | events | 否 | 否 |
> | impactedResources | 否 | 否 |
> | metadata | 否 | 否 |
> | 通知 | 否 | 否 |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | deployments | 是 | 否 |
> | deployments / operations | 否 | 否 |
> | deploymentScripts | 是 | 是 |
> | deploymentScripts / logs | 否 | 否 |
> | 链接 | 否 | 否 |
> | notifyResourceJobs | 否 | 否 |
> | providers | 否 | 否 |
> | resourceGroups | 是 | 否 |
> | subscriptions | 否 | 否 |
> | tenants | 否 | 否 |

<!--Not Available on  ## Microsoft.SaaS-->

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | jobcollections | 是 | 是 |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | resourceHealthMetadata | 否 | 否 |
> | searchServices | 是 | 是 |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | adaptiveNetworkHardenings | 否 | 否 |
> | advancedThreatProtectionSettings | 否 | 否 |
> | alerts | 否 | 否 |
> | allowedConnections | 否 | 否 |
> | applicationWhitelistings | 否 | 否 |
> | assessmentMetadata | 否 | 否 |
> | assessments | 否 | 否 |
> | autoDismissAlertsRules | 否 | 否 |
> | automations | 是 | 是 |
> | AutoProvisioningSettings | 否 | 否 |
> | Compliances | 否 | 否 |
> | dataCollectionAgents | 否 | 否 |
> | deviceSecurityGroups | 否 | 否 |
> | discoveredSecuritySolutions | 否 | 否 |
> | externalSecuritySolutions | 否 | 否 |
> | InformationProtectionPolicies | 否 | 否 |
> | iotSecuritySolutions | 是 | 是 |
> | iotSecuritySolutions / analyticsModels | 否 | 否 |
> | iotSecuritySolutions / analyticsModels / aggregatedAlerts | 否 | 否 |
> | iotSecuritySolutions / analyticsModels / aggregatedRecommendations | 否 | 否 |
> | jitNetworkAccessPolicies | 否 | 否 |
> | networkData | 否 | 否 |
> | 策略 | 否 | 否 |
> | pricings | 否 | 否 |
> | regulatoryComplianceStandards | 否 | 否 |
> | regulatoryComplianceStandards / regulatoryComplianceControls | 否 | 否 |
> | regulatoryComplianceStandards / regulatoryComplianceControls / regulatoryComplianceAssessments | 否 | 否 |
> | securityContacts | 否 | 否 |
> | securitySolutions | 否 | 否 |
> | securitySolutionsReferenceData | 否 | 否 |
> | securityStatuses | 否 | 否 |
> | securityStatusesSummaries | 否 | 否 |
> | serverVulnerabilityAssessments | 否 | 否 |
> | 设置 | 否 | 否 |
> | subAssessments | 否 | 否 |
> | 任务 | 否 | 否 |
> | topologies | 否 | 否 |
> | workspaceSettings | 否 | 否 |

<!--Not Available on  ## Microsoft.SecurityGraph-->
<!--Not Available on  ## Microsoft.SecurityInsights-->

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | namespaces | 是 | 否 |
> | namespaces / authorizationrules | 否 | 否 |
> | namespaces / disasterrecoveryconfigs | 否 | 否 |
> | namespaces / eventgridfilters | 否 | 否 |
> | namespaces / networkrulesets | 否 | 否 |
> | namespaces / queues | 否 | 否 |
> | namespaces / queues / authorizationrules | 否 | 否 |
> | namespaces / topics | 否 | 否 |
> | namespaces / topics / authorizationrules | 否 | 否 |
> | namespaces / topics / subscriptions | 否 | 否 |
> | namespaces / topics / subscriptions / rules | 否 | 否 |
> | premiumMessagingRegions | 否 | 否 |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | applications | 是 | 是 |
> | clusters | 是 | 是 |
> | clusters/applications | 否 | 否 |
> | containerGroups | 是 | 是 |
> | containerGroupSets | 是 | 是 |
> | edgeclusters | 是 | 是 |
> | edgeclusters / applications | 否 | 否 |
> | networks | 是 | 是 |
> | secretstores | 是 | 是 |
> | secretstores / certificates | 否 | 否 |
> | secretstores / secrets | 否 | 否 |
> | volumes | 是 | 是 |

<!--Not Available on ## Service Fabric Mesh-->
<!--Not Available on ## Microsoft.Services-->

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | SignalR | 是 | 是 |
> | SignalR / eventGridFilters | 否 | 否 |

<!--Not Available on ## Microsoft.SiteRecovery-->
<!--Not Available on ## Microsoft.Solutions-->

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | managedInstances | 是 | 是 |
> | managedInstances / databases | 否 | 否 |
> | managedInstances / databases / backupShortTermRetentionPolicies | 否 | 否 |
> | managedInstances / databases / schemas / tables / columns / sensitivityLabels | 否 | 否 |
> | managedInstances / databases / vulnerabilityAssessments | 否 | 否 |
> | managedInstances / databases / vulnerabilityAssessments / rules / baselines | 否 | 否 |
> | managedInstances / encryptionProtector | 否 | 否 |
> | managedInstances / keys | 否 | 否 |
> | managedInstances / restorableDroppedDatabases / backupShortTermRetentionPolicies | 否 | 否 |
> | managedInstances / vulnerabilityAssessments | 否 | 否 |
> | servers | 是 | 是 |
> | servers / administrators | 否 | 否 |
> | servers / communicationLinks | 否 | 否 |
> | servers/databases | 是（请参见[下面的注释](#sqlnote)） | 是 |
> | servers / encryptionProtector | 否 | 否 |
> | servers / firewallRules | 否 | 否 |
> | servers / keys | 否 | 否 |
> | servers / restorableDroppedDatabases | 否 | 否 |
> | servers / serviceobjectives | 否 | 否 |
> | servers / tdeCertificates | 否 | 否 |
> | virtualClusters | 否 | 否 |

<a name="sqlnote" />

> [!NOTE]
> Master 数据库不支持标记，但其他数据库（包括 Azure SQL 数据仓库数据库）支持标记。 Azure SQL 数据仓库数据库必须处于活动（而非暂停）状态。

<!--Not Available on ## Microsoft.SqlVirtualMachine-->

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | storageAccounts | 是 | 是 |
> | storageAccounts / blobServices | 否 | 否 |
> | storageAccounts / fileServices | 否 | 否 |
> | storageAccounts / queueServices | 否 | 否 |
> | storageAccounts / services | 否 | 否 |
> | storageAccounts / services / metricDefinitions | 否 | 否 |
> | storageAccounts / tableServices | 否 | 否 |
> | usages | 否 | 否 |

<!--Not Available on  ## Microsoft.StorageCache-->
<!--Not Available on  ## Microsoft.StorageReplication-->
<!--Not Available on  ## Microsoft.StorageSync-->
<!--Not Available on  ## Microsoft.StorageSyncDev-->
<!--Not Available on  ## Microsoft.StorageSyncInt-->
<!--Not Available on ## Microsoft.StorSimple-->

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | streamingjobs | 是（见下方备注） | 是 |

> [!NOTE]
> Streamingjobs 运行时无法添加标记。 停止要添加标记的资源。

<!--Not Available on ## Microsoft.Subscription-->

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | environments | 是 | 否 |
> | environments / accessPolicies | 否 | 否 |
> | environments/eventsources | 是 | 否 |
> | environments / referenceDataSets | 是 | 否 |

<!--Not Available on ## Microsoft.VMwareCloudSimple-->

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | 资源类型 | 支持标记 | 在成本报表中标记 |
> | ------------- | ----------- | ----------- |
> | apiManagementAccounts | 否 | 否 |
> | apiManagementAccounts / apiAcls | 否 | 否 |
> | apiManagementAccounts / apis | 否 | 否 |
> | apiManagementAccounts / apis / apiAcls | 否 | 否 |
> | apiManagementAccounts / apis / connectionAcls | 否 | 否 |
> | apiManagementAccounts / apis / connections | 否 | 否 |
> | apiManagementAccounts / apis / connections / connectionAcls | 否 | 否 |
> | apiManagementAccounts / apis / localizedDefinitions | 否 | 否 |
> | apiManagementAccounts / connectionAcls | 否 | 否 |
> | apiManagementAccounts / connections | 否 | 否 |
> | billingMeters | 否 | 否 |
> | certificates | 是 | 是 |
> | connectionGateways | 是 | 是 |
> | connections | 是 | 是 |
> | customApis | 是 | 是 |
> | deletedSites | 否 | 否 |
> | functions | 否 | 否 |
> | hostingEnvironments | 是 | 是 |
> | hostingEnvironments / multiRolePools | 否 | 否 |
> | hostingEnvironments / workerPools | 否 | 否 |
> | publishingUsers | 否 | 否 |
> | 建议 | 否 | 否 |
> | resourceHealthMetadata | 否 | 否 |
> | runtimes | 否 | 否 |
> | serverFarms | 是 | 是 |
> | serverFarms / eventGridFilters | 否 | 否 |
> | sites | 是 | 是 |
> | sites / config  | 否 | 否 |
> | sites / eventGridFilters | 否 | 否 |
> | sites / hostNameBindings | 否 | 否 |
> | sites / networkConfig | 否 | 否 |
> | sites/premieraddons | 是 | 是 |
> | sites/slots | 是 | 是 |
> | sites / slots / eventGridFilters | 否 | 否 |
> | sites / slots / hostNameBindings | 否 | 否 |
> | sites / slots / networkConfig | 否 | 否 |
> | sourceControls | 否 | 否 |
> | validate | 否 | 否 |
> | verifyHostingEnvironmentVnet | 否 | 否 |

<!--Not Available on  ## Microsoft.WindowsDefenderATP-->
<!--Not Available on  ## Microsoft.WindowsIoT-->
<!--Not Available on  ## Microsoft.WorkloadMonitor-->

## <a name="next-steps"></a>后续步骤

若要了解如何将标记应用于资源，请参见[使用标记来组织 Azure 资源](tag-resources.md)。

<!-- Update_Description: update meta properties, wording update, update link -->