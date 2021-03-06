---
title: Azure Cosmos DB 查询语言中的 GetCurrentDateTime
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 GetCurrentDateTime。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 32fa92a04aa054e17fe47f2040d0ef721a3e355b
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914733"
---
# <a name="getcurrentdatetime-azure-cosmos-db"></a>GetCurrentDateTime (Azure Cosmos DB)
 以 ISO 8601 字符串形式返回当前 UTC（协调世界时）日期和时间。

## <a name="syntax"></a>语法

```sql
GetCurrentDateTime ()
```

## <a name="return-types"></a>返回类型

  以 `YYYY-MM-DDThh:mm:ss.sssZ` 格式返回当前 UTC 日期和时间 ISO 8601 字符串值，其中：

  |||
  |-|-|
  |YYYY|四位数的年份|
  |MM|两位数的月份（01 = 1 月，依此类推。）|
  |DD|两位数的月份日期（01 到 31）|
  |T|时间元素开头的符号|
  |hh|两位数小时（00 到 23）|
  |MM|两位数分钟（00 到 59）|
  |ss|两位数秒（00 到 59）|
  |.sss|三位数的秒小数部分|
  |Z|UTC（协调世界时）指示符||

  有关 ISO 8601 格式的详细信息，请参阅 [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601)

## <a name="remarks"></a>备注

  GetCurrentDateTime() 是非确定性的函数。 

  返回的结果为 UTC。

## <a name="examples"></a>示例

  以下示例演示如何使用 GetCurrentDateTime() 内置函数获取当前 UTC 日期时间。

```sql
SELECT GetCurrentDateTime() AS currentUtcDateTime
```  

 下面是示例结果集。

```json
[{
  "currentUtcDateTime": "2019-05-03T20:36:17.784Z"
}]  
```  

## <a name="next-steps"></a>后续步骤

- [日期和时间函数 Azure Cosmos DB](sql-query-date-time-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query get current datetime  -->
<!--New.date: 10/28/2019-->