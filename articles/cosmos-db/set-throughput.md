---
title: 在 Azure Cosmos 容器和数据库上预配吞吐量
description: 了解如何为 Azure Cosmos 容器和数据库设置预配的吞吐量。
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 08/12/2019
ms.date: 12/16/2019
ms.openlocfilehash: 13f39424b1f5e7fdb975d067dc6c38acdeed43cc
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79291881"
---
# <a name="provision-throughput-on-containers-and-databases"></a>在容器和数据库上预配吞吐量

Azure Cosmos 数据库是一组容器的管理单元。 数据库包含一组不限架构的容器。 Azure Cosmos 容器是吞吐量和存储的缩放单元。 容器跨 Azure 区域中的一组计算机水平分区，并分布在与 Azure Cosmos 帐户关联的所有 Azure 中国区域中。

使用 Azure Cosmos DB 时，可以在两个粒度级别预配吞吐量：

- Azure Cosmos 容器
- Azure Cosmos 数据库

## <a name="set-throughput-on-a-container"></a>对容器设置吞吐量  

在 Azure Cosmos 容器上预配的吞吐量专门保留给该容器使用。 容器始终可获得预配的吞吐量。 对容器预配的吞吐量有 SLA 提供的经济保障。 若要了解如何在容器上配置吞吐量，请参阅[在 Azure Cosmos 容器上预配吞吐量](how-to-provision-container-throughput.md)。

在容器上设置预配吞吐量是最频繁使用的选项。 可以通过使用[请求单位 (RU)](request-units.md) 预配任意数量的吞吐量来弹性缩放容器的吞吐量。 

为容器预配的吞吐量在其物理分区之间均匀分布。假设有一个适当的分区键，它在物理分区之间均匀分配逻辑分区，那么吞吐量也均匀分布在容器的所有逻辑分区上。 无法选择性地指定逻辑分区的吞吐量。 由于某个容器的一个或多个逻辑分区由物理分区托管，因此，物理分区专属于该容器，并支持对该容器预配的吞吐量。 

如果逻辑分区上运行的工作负荷消耗的吞吐量超过了分配给该逻辑分区的吞吐量，操作将受到速率限制。 出现速率限制时，可以增大整个容器的预配吞吐量，或重试操作。 有关分区的详细信息，请参阅[逻辑分区](partition-data.md)。

如果你希望容器的性能有保证，则我们建议以容器粒度配置吞吐量。

下图显示了物理分区如何托管容器的一个或多个逻辑分区：

![物理分区](./media/set-throughput/resource-partition.png)

## <a name="set-throughput-on-a-database"></a>对数据库设置吞吐量

对 Azure Cosmos 数据库预配吞吐量时，在该数据库中的所有容器（称作共享的数据库容器）之间共享吞吐量。 一种例外是在数据库中的特定容器上指定了预配的吞吐量。 在容器之间共享数据库级预配吞吐量相当于在计算机群集上托管数据库。 由于数据库中的所有容器共享一台计算机上的可用资源，因此，任何特定容器的性能自然不可预测。 若要了解如何在数据库上配置预配吞吐量，请参阅[在 Azure Cosmos 数据库上配置预配吞吐量](how-to-provision-database-throughput.md)。

在 Azure Cosmos 数据库上设置吞吐量可保证随时能够获得该数据库的预配吞吐量。 由于数据库中的所有容器共享预配的吞吐量，因此，Azure Cosmos DB 不会针对该数据库中的特定容器提供任何可预测的吞吐量保证。 特定容器可获得的吞吐量部分取决于：

* 容器数量。
* 为各个容器选择的分区键。
* 工作负荷在容器的各个逻辑分区之间的分布形式。 

若要在多个容器之间共享吞吐量，而不希望将吞吐量专门提供给任何特定的容器使用，则我们建议对数据库配置吞吐量。 

以下示例演示了最适合在数据库级别的哪个位置预配吞吐量：

* 对于多租户应用程序来说，在一组容器之间共享数据库的预配吞吐量非常有用。 每个用户可由不同的 Azure Cosmos 容器表示。

* 将 VM 群集或本地物理服务器中托管的 NoSQL 数据库（例如 MongoDB 或 Cassandra）迁移到 Azure Cosmos DB 时，在一组容器之间共享数据库的预配吞吐量非常有利。 可将针对 Azure Cosmos 数据库配置的预配吞吐量视为在逻辑上等同于（但更具成本效益和弹性）MongoDB 或 Cassandra 群集的计算容量。  

必须使用[分区键](partition-data.md)创建在具有预配吞吐量的数据库内创建的所有容器。 在任意给定的时间点，分配给数据库中容器的吞吐量将分布在该容器的所有逻辑分区中。 如果有容器共享在数据库上配置的预配吞吐量，则无法选择性地将吞吐量应用到特定的容器或逻辑分区。 

如果逻辑分区上的工作负荷消耗的吞吐量超过了分配给特定逻辑分区的吞吐量，操作将受到速率限制。 出现速率限制时，可以增大整个数据库的吞吐量，或重试操作。 有关分区的详细信息，请参阅[逻辑分区](partition-data.md)。

对某个数据库预配的吞吐量可由该数据库内的容器共享。 数据库级共享吞吐量中的每个新容器将需要 100 RU/秒。 预配包含共享数据库产品/服务的容器时：

* 每 25 个容器分组到一个分区集中，并且在分区集中的容器之间共享数据库吞吐量 (D)。 如果数据库中最多有 25 个容器，并且在任何时间点，如果你只使用一个容器，则该容器可使用的吞吐量为“D”吞吐量的最大值。

* 对于在 25 个容器之后创建的每个新容器，将创建一个新的分区集，并在创建的新分区集之间分配数据库吞吐量（即 2 个分区集是 D/2，3 个分区集是 D/3…）。 在任何时间点，如果仅使用数据库中的一个容器，则该容器可以分别使用（D/2, D/3, D/4… 吞吐量）的最大值。 鉴于吞吐量降低，建议你在一个数据库中创建的容器不超过 25 个。

**示例**

* 如果创建一个名为“MyDB”的数据库，其预配置吞吐量为 10000 RU/秒。

* 如果在“MyDB”下预配 25 个容器，则所有容器都将分组到一个分区集中。 在任何时间点，如果仅使用数据库中的一个容器，那么它最多可以使用 10000 RU/秒 (D)。

* 预配第 26 个容器时，将创建一个新的分区集，并且吞吐量将在这两个分区集之间平均分配。 因此，在任何时间点，如果仅使用数据库中的一个容器，那么它最多可以使用 5000 RU/秒 (D/2)。 因为有两个分区集，所以吞吐量可共享性因子将分成 D/2。

    下图以图形方式演示了前面的示例：

    ![数据库级吞吐量的可共享性因子](./media/set-throughput/database-level-throughput-shareability-factor.png)

如果工作负荷涉及到删除数据库中的所有集合并重新创建集合，则我们建议删除空数据库，再重新创建新的数据库，然后创建集合。 下图显示了物理分区如何托管属于数据库中不同容器的一个或多个逻辑分区：

![物理分区](./media/set-throughput/resource-partition2.png)

## <a name="set-throughput-on-a-database-and-a-container"></a>对数据库和容器设置吞吐量

可以合并两个模型。 同时对数据库和容器预配吞吐量。 以下示例演示如何对 Azure Cosmos 数据库和容器预配吞吐量：

* 可以创建预配吞吐量为“K”RU 的名为“Z”的 Azure Cosmos 数据库。   
* 接下来，在该数据库中创建名为 A、B、C、D、E 的五个容器。      创建容器 B 时，请确保启用“为此容器预配专用吞吐量”  选项，并在此容器上显式配置“P”  个 RU 的预配吞吐量。 请注意，只有在创建数据库和容器时，才能配置共享吞吐量和专用吞吐量。 

    ![在容器级别设置吞吐量](./media/set-throughput/coll-level-throughput.png)

* “K”RU 吞吐量在 A、C、D、E 这四个容器之间共享。      提供给 A、C、D 或 E 的确切吞吐量各不相同。     每个容器的吞吐量没有 SLA。
* 保证名为 B 的容器始终可以获得“P”RU 吞吐量。   该容器有 SLA 的保障。

> [!NOTE]
> 具有预配吞吐量的容器无法转换为共享的数据库容器。 反之，共享的数据库容器无法转换为具有专用吞吐量的容器。

## <a name="update-throughput-on-a-database-or-a-container"></a>在数据库或容器上更新吞吐量

创建 Azure Cosmos 容器或数据库后，可以更新预配的吞吐量。 对于可以在数据库或容器上配置的最大预配吞吐量，没有任何限制。 最小预配吞吐量取决于以下因素： 

* 曾经存储在容器中的最大数据大小
* 曾经在容器上预配的最大吞吐量
* 曾经在数据库中创建的具有共享吞吐量的 Azure Cosmos 容器的最大数目。 

可以通过 SDK 以编程方式检索容器或数据库的最小吞吐量，也可以在 Azure 门户中查看值。 使用 .NET SDK 时，可以通过 [DocumentClient.ReplaceOfferAsync](https://docs.azure.cn/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync?view=azure-dotnet) 方法缩放预配的吞吐量值。 使用 Java SDK 时，可以通过 [RequestOptions.setOfferThroughput](sql-api-java-samples.md#offer-examples) 方法缩放预配的吞吐量值。 

使用 .NET SDK 时，可以通过 [DocumentClient.ReadOfferAsync](https://docs.azure.cn/dotnet/api/microsoft.azure.documents.client.documentclient.readofferasync?view=azure-dotnet) 方法检索容器或数据库的最小吞吐量。 

可以随时缩放容器或数据库的预配吞吐量。 执行缩放操作以增加吞吐量时，由于系统任务的原因，可能需要更长的时间来预配所需的资源。 可以在 Azure 门户中检查缩放操作的状态，也可以使用 SDK 以编程方式检查。 使用 .NET SDK 时，可以通过 `DocumentClient.ReadOfferAsync` 方法获取缩放操作的状态。

## <a name="comparison-of-models"></a>模型比较

|**参数** |**对数据库的预配吞吐量** |**对容器预配的吞吐量**|
|---------|---------|---------|
|最小 RU 数 |400（前四个容器之后的每个容器均需要至少每秒 100 RU 的吞吐量。） |400|
|每个容器的最小 RU 数|100|400|
|最大 RU 数|对于数据库无限。|对于容器无限。|
|分配或提供给特定容器的 RU 数|无保证。 为给定容器分配的 RU 数取决于多种属性。 属性可以是为共享吞吐量的容器选择的分区键、工作负荷的分布，以及容器的数量。 |对容器配置的所有 RU 专门保留给该容器使用。|
|容器的最大存储|不受限制。|不受限制。|
|容器的每个逻辑分区的最大吞吐量|10K RU|10K RU|
|容器的每个逻辑分区的最大存储（数据 + 索引）|10 GB|10 GB|

## <a name="next-steps"></a>后续步骤

* 详细了解[逻辑分区](partition-data.md)。
* 了解[如何对 Azure Cosmos 容器预配吞吐量](how-to-provision-container-throughput.md)。
* 了解[如何对 Azure Cosmos 数据库预配吞吐量](how-to-provision-database-throughput.md)。

<!-- Update_Description: udpate meta properties, wording update -->
