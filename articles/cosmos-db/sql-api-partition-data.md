---
title: Azure Cosmos DB 中的分区和缩放 | Azure
description: 了解分区在 Azure Cosmos DB 中的工作原理，分区和分区键的配置方式以及应用程序分区键的选取方法。
services: cosmos-db
author: rockboyfor
manager: digimobile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: conceptual
origin.date: 05/24/2017
ms.date: 12/03/2018
ms.author: v-yeche
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 72582599f5376c110739aa353bc312f7c6d79069
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52674698"
---
# <a name="partitioning-in-azure-cosmos-db-using-the-sql-api"></a>使用 SQL API 在 Azure Cosmos DB 中进行分区

[Azure Cosmos DB](../cosmos-db/introduction.md) 是一项多区域分布式多模型数据库服务，旨在帮助你实现快速、可预测的性能并根据应用程序的增长情况进行无缝扩展。 

本文概述了如何使用 SQL API 处理对 Cosmos DB 容器的分区。 请参阅[分区和水平缩放](../cosmos-db/partition-data.md)，大致了解使用任何 Azure Cosmos DB API 进行分区的概念和最佳做法。 

若要开始处理代码，请从 [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark) 下载项目。 

阅读本文后，能够回答以下问题：   

* Azure Cosmos DB 中的分区原理是什么？
* 如何配置 Azure Cosmos DB 中的分区
* 什么是分区键，以及如何为应用程序选取适当的分区键？

若要开始使用代码，请从 [Azure Cosmos DB 性能测试驱动程序示例](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark)下载项目。 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a>分区键

在 SQL API 中，可以通过 JSON 路径形式指定分区键定义。 下表显示分区键定义以及与每个定义相对应的值的示例。 分区键指定为路径，例如 `/department` 表示属性 department。 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p>分区键</p></td>
            <td valign="top"><p><strong>说明</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/department</p></td>
            <td valign="top"><p>对应于 doc.department 的值，其中 doc 指的是项。</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/properties/name</p></td>
            <td valign="top"><p>对应于 doc.properties.name 的值，其中 doc 指的是项（嵌套属性）。</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/id</p></td>
            <td valign="top"><p>对应于 doc.id 的值（ID 和分区键是相同属性）。</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/"department name"</p></td>
            <td valign="top"><p>对应于 doc["部门名称"] 的值，其中 doc 指的是项。</p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> 分区键的语法类似于索引策略路径的路径规范，关键差别在于路径对应于属性而不是值，即末尾没有通配符。 例如，会指定 /department /? 以在部门下为值编制索引，但指定 /department 作为分区键定义。 分区键以隐式的方式进行索引编制，而且不能使用索引策略覆盖从索引中排除它。
> 
> 

让我们看看所选的分区键如何影响应用程序的性能。

## <a name="working-with-the-azure-cosmos-db-sdks"></a>使用 Azure Cosmos DB SDK
Azure Cosmos DB 增加了对 [REST API 版本 2015-12-16](https://docs.microsoft.com/rest/api/cosmos-db/) 的自动分区支持。 为了创建已分区容器，必须在支持的 SDK 平台之一（.NET、Node.js、Java、Python、MongoDB）下载 SDK 版本 1.6.0 或更高版本。 

### <a name="creating-containers"></a>创建容器
下面的示例演示的 .NET 代码片段用于创建容器，以存储吞吐量为 20,000 个请求单位/秒的设备遥测数据。 SDK 将设置 OfferThroughput 值（其反过来将设置 REST API 中的 `x-ms-offer-throughput` 请求标头）。 在这里，请将 `/deviceId` 设为分区键。 所选分区键随容器元数据（如名称和索引策略）的其余部分一起保存。

对于此示例，你选取了 `deviceId`，因为你知道：(a) 由于存在大量的设备，写入可以跨分区均匀地分步并且你可以扩展数据库以引入海量数据，(b) 许多请求（如提取设备最近读取内容）仅限于单个 deviceId，并且可以从单个分区进行检索。

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

// Container for device telemetry. Here the property deviceId will be used as the partition key to 
// spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
// sorting against any number or string property.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

此方法可对 Cosmos DB 进行 REST API 调用，且该服务基于所请求的吞吐量预配分区数。 根据性能需求的变化，可以更改一个或一组容器的吞吐量。 

### <a name="reading-and-writing-items"></a>读取和写入项
现在，让我们将数据插入 Cosmos DB。 以下示例类包含设备读取和对 CreateDocumentAsync 的调用，目的是将新设备读数插入到容器中。 下面是一个使用 SQL API 的示例代码块：

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```

请按分区键和 ID 读取项，对其进行更新，最后通过分区键和 ID 将其删除。读取包括 PartitionKey 值（对应 REST API 中的 `x-ms-documentdb-partitionkey` 请求标头）。

```csharp
// Read document. Needs the partition key and the ID to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;

// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);

// Delete document. Needs partition key
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="querying-partitioned-containers"></a>查询已分区容器
在已分区容器中查询数据时，Cosmos DB 会自动将查询路由到筛选器（如果有）中所指定分区键值对应的分区。 例如，此查询将只路由到包含分区键“XMS-0001”的分区。

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```

下面的查询在分区键 (DeviceId) 上没有筛选器，并且以扇形展开到针对分区索引执行该查询的所有分区。 必须指定 EnableCrossPartitionQuery（REST API 中的 `x-ms-documentdb-query-enablecrosspartition`）以使 SDK 跨分区执行查询。

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

从 SDK 1.12.0 及更高版本开始，Cosmos DB 支持使用 SQL 对已分区容器执行[聚合函数](how-to-sql-query.md#Aggregates) `COUNT`、`MIN`、`MAX`、`AVG` 操作。 查询必须包括单个聚合运算符，并且必须在投影中包括单个值。

### <a name="parallel-query-execution"></a>并行查询执行
Cosmos DB SDK 1.9.0 及更高版本支持并行查询执行选项，这些选项可用于对已分区集合执行低延迟查询，即使在这些查询需要处理大量分区时，也是如此。 例如，以下查询配置为跨分区并行运行。

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```

可以通过调整以下参数来管理并行查询执行：

* 通过设置 `MaxDegreeOfParallelism`，可以控制并行度，即，与容器的分区同时进行的网络连接的最大数量。 如果将此属性设置为 -1，则由 SDK 管理并行度。 如果 `MaxDegreeOfParallelism` 未指定或设置为 0（默认值），则与容器的分区的网络连接只有一个。
* 通过设置 `MaxBufferedItemCount`，可以权衡查询延迟和客户端内存使用率。 如果省略此参数或将此属性设置为 -1，则由 SDK 管理并行查询执行过程中缓冲的项目数。

如果给定相同状态的集合，并行查询会以与串行执行相同的顺序返回结果。 执行包含排序（ORDER BY 和/或 TOP）的跨分区查询时，Azure Cosmos DB SDK 跨分区发出并行查询，并合并客户端中的部分排序结果，以生成全局范围内有序的结果。

### <a name="executing-stored-procedures"></a>执行存储过程
还可以对具有相同设备 ID 的文档执行原子事务，例如，如果要在单个项目中维护聚合或设备的最新状态。 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

下一节介绍如何从单分区容器移动到已分区容器。

## <a name="next-steps"></a>后续步骤
本文概述了如何使用 SQL API 对 Azure Cosmos DB 容器进行分区。 另请参阅[分区和水平缩放](../cosmos-db/partition-data.md)，大致了解使用任何 Azure Cosmos DB API 进行分区的概念和最佳做法。 

* 使用 Azure Cosmos DB 执行规模和性能测试。 有关示例，请参阅[使用 Azure Cosmos DB 执行性能和规模测试](performance-testing.md)。
* 使用 [SDK](sql-api-sdk-dotnet.md) 或 [REST API](https://docs.microsoft.com/rest/api/cosmos-db/) 的编码入门
* 了解 [Azure Cosmos DB 中的预配吞吐量](request-units.md)

<!-- Update_Description: wording update, update link -->