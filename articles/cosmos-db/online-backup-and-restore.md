---
title: 使用 Azure Cosmos DB 进行联机备份和还原 | Azure
description: 了解如何在 Azure Cosmos DB 数据库上执行自动备份和还原。
keywords: 备份和还原、联机备份
services: cosmos-db
author: rockboyfor
manager: digimobile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
origin.date: 11/15/2017
ms.date: 11/05/2018
ms.author: v-yeche
ms.openlocfilehash: 764a99043a73202cfecc12166c17e83589652cae
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52659011"
---
# <a name="automatic-online-backup-and-restore-with-azure-cosmos-db"></a>使用 Azure Cosmos DB 进行自动联机备份和还原
Azure Cosmos DB 可以定期自动备份所有数据。 自动备份不会影响数据库操作的性能或可用性。 所有备份单独存储在另一个存储服务中并在多区域复制，以便针对区域性灾难进行复原。 如果意外删除了 Cosmos DB 容器并且之后需要进行数据恢复，那么自动备份将是合适的方案。  

本文首先快速回顾一个 Cosmos DB 的数据冗余和可用性，并介绍备份。 

## <a name="high-availability-with-cosmos-db---a-recap"></a>Cosmos DB 的高可用性 - 回顾
Cosmos DB 旨在实现数据[多区域分发](distribute-data-globally.md) - 它允许缩放多个 Azure 区域中的吞吐量，并提供策略驱动的故障转移和透明的多宿主 API。 Azure Cosmos DB 为所有单区域帐户和具有松散一致性的所有多区域帐户提供 [99.99% 的可用性 SLA](https://www.azure.cn/support/sla/cosmos-db)，为所有多区域数据库帐户提供 99.999% 的读取可用性。 Azure Cosmos DB 中的所有写入在确认到客户端之前，都会通过本地数据中心内的副本仲裁持久提交到本地磁盘。 Cosmos DB 的高可用性依赖本地存储，而不依赖任何外部存储技术。 此外，如果数据库帐户与多个 Azure 区域关联，则还会将写入内容复制到其他区域。 若要调整吞吐量规模并以低延迟访问数据，可以拥有任意数量的与数据库帐户关联的读取区域。 在每个读取区域中，（复制的）数据持久保存在副本集中。  

如下图所示，单个 Cosmos DB 容器是[水平分区](partition-data.md)的。 下图中的一个圆圈表示一个“分区”，每个分区通过副本集实现高可用性。 这是单个 Azure 区域中的本地分布（以 X 轴表示）。 此外，每个分区（含有其相应的副本集）都将在与数据库帐户关联的多个区域中进行多区域分发（例如，此图中的三个区域 - 中国东部、中国北部和中国东部 2）。 “分区集”是多区域分发的实体，由数据在每个区域中的多个副本组成（以 Y 轴表示）。 可将优先级分配到与数据库帐户关联的区域，Cosmos DB 将透明故障转移到下一区域，以防灾难发生。 还可以手动模拟故障转移，以测试应用程序的端到端可用性。  

<!--Notice: Change Central India to China East 2-->

下图演示了 Cosmos DB 的高度冗余。

<!-- Not Available on the first images of the Demo -->

![Cosmos DB 的高度冗余](./media/online-backup-and-restore/global-distribution.png)

## <a name="full-automatic-online-backups"></a>完整的自动化联机备份
糟糕，我删除了容器或数据库！ 使用 Cosmos DB，不仅仅是数据，还有数据备份都能获得高度冗余，可以弹性应对区域性的灾难。 这些自动备份当前大约每四小时进行一次，并且始终存储最新的两个备份。 如果数据意外删除或损坏，请在八小时内[联系 Azure 支持部门](https://www.azure.cn/support/contact/)。 

这些备份不会影响数据库操作的性能或可用性。 Cosmos DB 在后台创建备份，不使用预配的 RU，不影响性能，也不影响数据库的可用性。 

不同于存储在 Cosmos DB 中的数据，自动备份存储在 Azure Blob 存储服务中。 为了保证低延迟/高效上传，备份快照会上传到某个 Azure Blob 存储实例，该实例位于 Cosmos DB 数据库帐户当前写入区域所在的同一个区域。 此外，为了弹性应对区域性灾难，还会通过异地冗余存储 (GRS) 将 Azure Blob 存储中的每个备份数据快照复制到另一个区域。 下图显示整个 Cosmos DB 容器（在本示例中为中国北部的所有三个主分区）已在中国北部的远程 Azure Blob 存储帐户中备份，并通过 GRS 复制到中国东部。 

下图演示了 GRS Azure 存储中所有 Cosmos DB 实体的定期完整备份。

![GRS Azure 存储中所有 Cosmos DB 实体的定期完整备份](./media/online-backup-and-restore/automatic-backup.png)

<!-- Update the images on Mooncake-->

## <a name="backup-retention-period"></a>备份保留期
如上所述，Azure Cosmos DB 在分区级别每四小时创建一次数据快照。 在任何给定时间，只保留最后两个快照。 不过，如果删除了容器/数据库，Azure Cosmos DB 将给定容器/数据库中所有已删除分区的现有快照保留 30 天。

对于 SQL API，如果你要维护自己的快照，可以使用以下选项来实现：

* 使用 Azure Cosmos DB [数据迁移工具](import-data.md#export-to-json-file)中的“导出到 JSON”选项来计划其他备份。

<!-- Not Available on [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)-->
* 使用 Azure Cosmos DB [更改源](change-feed.md)定期读取数据来进行完整备份，另外，对于增量更改，将其移动到你的 blob 目标。 

* 若要管理温备份，可以定期从“更改源”读取数据并延迟将其写入到另一集合中。 这可以确保你无需还原数据，并且可以立即查看数据是否有问题。 

> [!NOTE]
> 如果“在数据库级别为一组容器预配吞吐量”，请记得在整个数据库帐户级别进行还原操作。 如果无意中删除了容器，还需要确保在 8 小时内联系支持团队。 如果未在 8 小时内联系支持团队，则无法还原数据。

<!-- Not Available on table/graph -->

## <a name="restoring-a-database-from-an-online-backup"></a>从联机备份还原数据库

如果意外删除了数据库或容器，可以[提交支持票证](https://www.azure.cn/support/support-azure/)或[联系 Azure 支持](https://www.azure.cn/support/contact/)，以便从上一次自动备份中还原数据。 Azure 支持仅适用于选定计划（例如标准计划、发开人员计划），不适用于基本计划。 若要了解不同的支持方案，请参阅 [Azure 支持计划](https://www.azure.cn/support/plans/)页。 

如果由于数据损坏（包括删除了容器中的文档）而需要还原数据库，请参阅[处理数据损坏](#handling-data-corruption)，因为需要采取额外步骤，防止损坏的数据覆盖现有备份。 对于要还原备份的特定快照，Cosmos DB 要求数据在该快照的备份周期的持续时间内可用。

> [!NOTE]
> 集合或数据库只有在客户显式提出请求时才能还原。 客户负责在协调数据后立即删除容器或数据库。 如果不删除已还原的数据库或集合，它们将在请求单位、存储和出口方面产生成本。

## <a name="handling-data-corruption"></a>处理数据损坏

Azure Cosmos DB 保留数据库帐户中每个分区的最后两个备份。 意外删除容器（文档的集合）或数据库时，该模型可以很好地处理这种情况，因为它可以还原其中一个最新版本。 但是，在用户可能引入数据损坏问题的情况下，Azure Cosmos DB 可能不知道数据损坏，并且损坏可能已覆盖现有备份。

一旦检测到损坏情况，用户应删除损坏的容器（集合），以防止备份被损坏的数据覆盖。 最重要的是，联系 Azure 支持并通过票证提交严重性为 2 的具体请求。 
<!-- Not Avaiable on table and Graph -->
<!-- Not Available on global support request-->

## <a name="next-steps"></a>后续步骤

若要在多个数据中心复制数据库，请参阅[使用 Cosmos DB 多区域分配数据](distribute-data-globally.md)。 

若要联系 Azure 支持，请 [从 Azure 门户开具票证](https://www.azure.cn/support/support-azure/)。

<!--Update_Description: update meta properties, wording update -->