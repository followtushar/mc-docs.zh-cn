---
title: 使用 Azure 数据工厂从 SAP HANA 复制数据
description: 了解如何通过在 Azure 数据工厂管道中使用复制活动，将数据从 SAP HANA 复制到支持的接收器数据存储。
services: data-factory
documentationcenter: ''
author: WenJason
manager: digimobile
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 09/02/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.openlocfilehash: 517055ecb201e20d0cf70f5d4a0c39377dccd512
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75623731"
---
# <a name="copy-data-from-sap-hana-using-azure-data-factory"></a>使用 Azure 数据工厂从 SAP HANA 复制数据
本文概述了如何使用 Azure 数据工厂中的复制活动从 SAP HANA 数据库复制数据。 它是基于概述复制活动总体的[复制活动概述](copy-activity-overview.md)一文。

>[!TIP]
>若要了解 ADF 对 SAP 数据集成方案的总体支持，请参阅[使用 Azure 数据工厂进行 SAP 数据集成白皮书](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf)，其中包含详细介绍、比较和指导。

## <a name="supported-capabilities"></a>支持的功能

以下活动支持此 SAP HANA 连接器：

- 带有[支持的源或接收器矩阵](copy-activity-overview.md)的[复制活动](copy-activity-overview.md)
- [Lookup 活动](control-flow-lookup-activity.md)

可以将数据从 SAP HANA 数据库复制到任何支持的接收器数据存储。 有关复制活动支持作为源/接收器的数据存储列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)表。

具体而言，此 SAP HANA 连接器支持：

- 从任何版本的 SAP HANA 数据库复制数据。
- 从 **HANA 信息模型**（如分析和计算视图）和**行/列表**中复制数据。
- 使用**基本**或 **Windows** 身份验证复制数据。

> [!TIP]
> 要将数据复制到  SAP HANA 数据存储，请使用泛型 ODBC 连接器。 有关详细信息，请参阅 [SAP HANA 接收器](connector-odbc.md#sap-hana-sink)。 注意：适用于 SAP HANA 连接器和 ODBC 连接器的链接服务采用不同的类型，因此不能重用。

## <a name="prerequisites"></a>先决条件

要使用此 SAP HANA 连接器，需要：

- 设置自承载集成运行时。 有关详细信息，请参阅[自承载集成运行时](create-self-hosted-integration-runtime.md)一文。
- 在集成运行时计算机上安装 SAP HANA ODBC 驱动程序。 可以从 [SAP 软件下载中心](https://support.sap.com/swdc)下载 SAP HANA ODBC 驱动程序。 使用关键字 **SAP HANA CLIENT for Windows** 进行搜索。

## <a name="getting-started"></a>入门

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

对于特定于 SAP HANA 连接器的数据工厂实体，以下部分提供有关用于定义这些实体的属性的详细信息。

## <a name="linked-service-properties"></a>链接服务属性

SAP HANA 链接的服务支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | type 属性必须设置为：**SapHana** | 是 |
| connectionString | 指定使用**基本身份验证**或 **Windows 身份验证**连接到 SAP HANA 时所需的信息。 请参阅以下示例。<br>在连接字符串中，服务器/端口是必需的（默认端口为 30015）。在使用基本身份验证时，用户名和密码是必需的。 有关其他高级设置，请参阅 [SAP HANA ODBC 连接属性](https://help.sap.com/viewer/0eec0d68141541d1b07893a39944924e/2.0.02/en-US/7cab593774474f2f8db335710b2f5c50.html)<br/>还可以将密码放在 Azure 密钥保管库中，并从连接字符串中拉取密码配置。 有关更多详细信息，请参阅[在 Azure Key Vault 中存储凭据](store-credentials-in-key-vault.md)一文。 | 是 |
| userName | 使用 Windows 身份验证时，请指定用户名。 示例： `user@domain.com` | 否 |
| password | 指定用户帐户的密码。 将此字段标记为 SecureString 以安全地将其存储在数据工厂中或[引用存储在 Azure Key Vault 中的机密](store-credentials-in-key-vault.md)。 | 否 |
| connectVia | 用于连接到数据存储的[集成运行时](concepts-integration-runtime.md)。 如[先决条件](#prerequisites)中所述，需要自承载集成运行时。 |是 |

**示例：使用基本身份验证**

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "connectionString": "SERVERNODE=<server>:<port (optional)>;UID=<userName>;PWD=<Password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**示例：使用 Windows 身份验证**

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "connectionString": "SERVERNODE=<server>:<port (optional)>;",
            "userName": "<username>", 
            "password": { 
                "type": "SecureString", 
                "value": "<password>" 
            } 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

如果使用的是具有以下有效负载的 SAP HANA 链接服务，它仍然按原样受支持，但建议使用新的版本。

**示例：**

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server>:<port (optional)>",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>数据集属性

有关可用于定义数据集的各部分和属性的完整列表，请参阅[数据集](concepts-datasets-linked-services.md)一文。 本部分提供 SAP HANA 数据集支持的属性列表。

支持使用以下属性从 SAP HANA 复制数据：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 数据集的 type 属性必须设置为：**SapHanaTable** | 是 |
| schema | SAP HANA 数据库中架构的名称。 | 否（如果指定了活动源中的“query”） |
| 表 | SAP HANA 数据库中表的名称。 | 否（如果指定了活动源中的“query”） |

**示例：**

```json
{
    "name": "SAPHANADataset",
    "properties": {
        "type": "SapHanaTable",
        "typeProperties": {
            "schema": "<schema name>",
            "table": "<table name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP HANA linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

如果使用 `RelationalTable` 类型数据集，该数据集仍按原样受支持，但我们建议今后使用新数据集。

## <a name="copy-activity-properties"></a>复制活动属性

有关可用于定义活动的各部分和属性的完整列表，请参阅[管道](concepts-pipelines-activities.md)一文。 本部分提供 SAP HANA 源支持的属性列表。

### <a name="sap-hana-as-source"></a>以 SAP HANA 作为源

若要从 SAP HANA 复制数据，复制活动的 **source** 节支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 复制活动 source 的 type 属性必须设置为：**SapHanaSource** | 是 |
| 查询 | 指定要从 SAP HANA 实例读取数据的 SQL 查询。 | 是 |
| packetSize | 指定网络数据包大小 (KB)，以便将数据拆分成多个块。 如果有大量的数据需要复制，则大多数情况下，提高数据包大小可以提高从 SAP HANA 读取数据的速度。 调整数据包大小时，建议进行性能测试。 | 否。<br>默认值为 2048 (2MB)。 |

**示例：**

```json
"activities":[
    {
        "name": "CopyFromSAPHANA",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP HANA input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SapHanaSource",
                "query": "<SQL query for SAP HANA>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

如果使用 `RelationalSource` 类型复制源，该源仍按原样受支持，但我们建议今后使用新源。

## <a name="data-type-mapping-for-sap-hana"></a>SAP HANA 的数据类型映射

从 SAP HANA 复制数据时，以下映射用于从 SAP HANA 数据类型映射到 Azure 数据工厂临时数据类型。 若要了解复制活动如何将源架构和数据类型映射到接收器，请参阅[架构和数据类型映射](copy-activity-schema-and-type-mapping.md)。

| SAP HANA 数据类型 | 数据工厂临时数据类型 |
| ------------------ | ------------------------------ |
| ALPHANUM           | String                         |
| BIGINT             | Int64                          |
| BINARY             | Byte[]                         |
| BINTEXT            | String                         |
| BLOB               | Byte[]                         |
| BOOL               | Byte                           |
| CLOB               | String                         |
| DATE               | DateTime                       |
| DECIMAL            | 小数                        |
| DOUBLE             | Double                         |
| FLOAT              | Double                         |
| INTEGER            | Int32                          |
| NCLOB              | String                         |
| NVARCHAR           | String                         |
| REAL               | Single                         |
| SECONDDATE         | DateTime                       |
| SHORTTEXT          | String                         |
| SMALLDECIMAL       | 小数                        |
| SMALLINT           | Int16                          |
| STGEOMETRYTYPE     | Byte[]                         |
| STPOINTTYPE        | Byte[]                         |
| TEXT               | String                         |
| TIME               | TimeSpan                       |
| TINYINT            | Byte                           |
| VARCHAR            | String                         |
| TIMESTAMP          | DateTime                       |
| VARBINARY          | Byte[]                         |

## <a name="lookup-activity-properties"></a>Lookup 活动属性

若要了解有关属性的详细信息，请查看 [Lookup 活动](control-flow-lookup-activity.md)。

## <a name="next-steps"></a>后续步骤
有关 Azure 数据工厂中复制活动支持作为源和接收器的数据存储的列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)。
