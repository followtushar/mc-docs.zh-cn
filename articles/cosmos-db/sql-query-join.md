---
title: Azure Cosmos DB 的 SQL JOIN 查询
description: 了解如何联接 Azure Cosmos DB 中的多个表以查询数据
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 05/17/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: 1a815161ec451dc1622061687a0db7c2f144ed0d
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028967"
---
# <a name="joins-in-azure-cosmos-db"></a>Azure Cosmos DB 中的联接

在关系数据库中，跨表联接是设计规范化架构的逻辑定理。 相比之下，SQL API 使用无架构项的反规范化数据模型，这在逻辑上等效于自联接。 

内联可获得参与联接的集的完整叉积。 N 向联接的结果是一个 N 元素元组集，其中，元组中的每个值都与参与联接的具有别名的集相关联，并且可以通过在其他子句中引用该别名进行访问。

## <a name="syntax"></a>语法

该语言支持语法 `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`。 此查询返回一组包含 `N` 值的元组。 每个元组拥有通过对它们相应的集遍历所有容器别名所产生的值。 

请看下面的 FROM 子句：`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  

让每个源定义 `input_alias1, input_alias2, …, input_aliasN`。 此 FROM 子句返回一个 N 元组集（包含 N 个值的元组）。 每个元组拥有通过对它们相应的集遍历所有容器别名所产生的值。  

示例 1 - 2 个源   

- 让 `<from_source1>` 的范围为容器，并表示集 {A, B, C}。  

- 让 `<from_source2>` 以文档为作用域，引用 input_alias1 并表示以下集：  

    {1, 2}，对于 `input_alias1 = A,`  

    若 `input_alias1 = B,`，表示集 {3}  

    {4, 5}，对于 `input_alias1 = C,`  

- FROM 子句 `<from_source1> JOIN <from_source2>` 将生成以下元组：  

    (`input_alias1, input_alias2`)：  

    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  

示例 2 - 3 个源   

- 让 `<from_source1>` 的范围为容器，并表示集 {A, B, C}。  

- 让 `<from_source2>` 以文档为作用域，引用 `input_alias1` 并表示以下集：  

    {1, 2}，对于 `input_alias1 = A,`  

    若 `input_alias1 = B,`，表示集 {3}  

    {4, 5}，对于 `input_alias1 = C,`  

- 让 `<from_source3>` 以文档为作用域，引用 `input_alias2` 并表示以下集：  

    {100, 200}，对于 `input_alias2 = 1,`  

    若 `input_alias2 = 3,`，表示集 {300}  

- FROM 子句 `<from_source1> JOIN <from_source2> JOIN <from_source3>` 将生成以下元组：  

    (input_alias1, input_alias2, input_alias3)：  

    (A, 1, 100)、(A, 1, 200)、(B, 3, 300)  

    > [!NOTE]
    > 对于 `input_alias1`、`input_alias2` 的其他值，由于缺少元组，因此 `<from_source3>` 没有返回任何值。  

示例 3 - 3 个源   

- 让 <from_source1> 的范围为容器，并表示集 {A, B, C}。  

- 让 `<from_source1>` 的范围为容器，并表示集 {A, B, C}。  

- 让 <from_source2> 以文档为作用域，引用 input_alias1 并表示以下集：  

    {1, 2}，对于 `input_alias1 = A,`  

    若 `input_alias1 = B,`，表示集 {3}  

    {4, 5}，对于 `input_alias1 = C,`  

- 让 `<from_source3>` 以 `input_alias1` 为作用域并表示以下集：  

    {100, 200}，对于 `input_alias2 = A,`  

    若 `input_alias2 = C,`，表示集 {300}  

- FROM 子句 `<from_source1> JOIN <from_source2> JOIN <from_source3>` 将生成以下元组：  

    (`input_alias1, input_alias2, input_alias3`)：  

    (A, 1, 100)、(A, 1, 200)、(A, 2, 100)、(A, 2, 200)、(C, 4, 300)、(C, 5, 300)  

    > [!NOTE]
    > 这生成了 `<from_source2>` 与 `<from_source3>` 之间的叉积，因为这两者的作用域是同一个 `<from_source1>`。  这生成了 4 个 (2x2) 具有值 A 的元组、0 个 (1x0) 具有值 B 的元组和 2 个 (2x1) 具有值 C 的元组。  

## <a name="examples"></a>示例

下面的示例演示了 JOIN 子句的工作原理。 在运行这些示例之前，请上传示例[系列数据](sql-query-getting-started.md#upload-sample-data)。 在以下示例中，由于源中每个项和空集的叉积为空，因此结果为空：

```sql
    SELECT f.id
    FROM Families f
    JOIN f.NonExistent
```

结果为：

```json
    [{
    }]
```

在以下示例中，联接是两个 JSON 对象、项根 `id` 和子根 `children` 之间的叉积。 `children` 是数组这一事实在联接中不起作用，因为查询处理的是作为 `children` 数组的单一根。 由于每个带有数组的项的叉积仅生成一个项，因此结果仅包含两个结果。

```sql
    SELECT f.id
    FROM Families f
    JOIN f.children
```

结果有：

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

下面的示例演示了更传统的联接：

```sql
    SELECT f.id
    FROM Families f
    JOIN c IN f.children
```

结果有：

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

JOIN 子句的 FROM 源是一个迭代器。 因此，以上示例中的流程为：  

1. 展开数组中的每个子元素 `c`。
2. 应用包含项 `f` 的根的叉积，该项包含已在第一个步骤中平展的每个子元素 `c`。
3. 最后，单独投影根对象 `f` `id` 属性。

第一个项 (`AndersenFamily`) 仅包含一个 `children` 元素，因此结果集仅包含单个对象。 第二个项 `WakefieldFamily` 包含两个 `children`，因此，叉积为每个 `children` 元素生成一个对象，共两个对象。 这两个项中的根字段会是相同的，正如在叉积中所预期的一样。

JOIN 子句真正实用的地方是通过以其他方式难以投影的形式基于叉积生成元组。 以下示例对元组组合进行筛选，让用户选择元组在整体上满足的条件。

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
```

结果有：

```json
    [
      {
        "familyName": "AndersenFamily",
        "childFirstName": "Henriette Thaulow",
        "petName": "Fluffy"
      },
      {
        "familyName": "WakefieldFamily",
        "childGivenName": "Jesse",
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

以下示例对前一个示例做了延伸，将会执行双重联接。 可将叉积视为下面所示的伪代码：

```
    for-each(Family f in Families)
    {
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName,
                  c.givenName AS childGivenName,
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }
```

`AndersenFamily` 中有一个孩子拥有一只宠物，因此叉积从此家庭生成了一行 (1\*1\*1)。 `WakefieldFamily` 中有两个孩子，其中只有一个孩子拥有宠物，但这个孩子拥有两只宠物。 叉积对此家庭生成了 1\*1\*2 = 2 行。

以下示例根据 `pet` 进行了额外的筛选，这排除了宠物名称不是 `Shadow` 的所有元组。 可以基于数组生成元组，根据元组的任意元素进行筛选以及投影元素的任何组合。

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"
```

结果有：

```json
    [
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

## <a name="next-steps"></a>后续步骤

- [入门](sql-query-getting-started.md)
- [Azure Cosmos DB .NET 示例](https://github.com/Azure/azure-cosmosdb-dotnet)
- [子查询](sql-query-subquery.md)

<!-- Update_Description: wording update, update link -->