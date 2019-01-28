---
title: Azure Data Lake Storage Gen2 的已知问题 | Microsoft Docs
description: 了解 Azure Data Lake Storage Gen2 的限制和已知问题
services: storage
author: WenJason
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
origin.date: 12/05/2018
ms.date: 01/21/2019
ms.author: v-jay
ms.openlocfilehash: c4ab7461e3818b3893f8d3a08d38fb9608783269
ms.sourcegitcommit: c3f2948c7350c71dd66228ccf10332e21b686030
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396997"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2 的已知问题

本文包含 Azure Data Lake Storage Gen2 的已知问题和暂时性限制。

## <a name="api-interoperability"></a>API 互操作性

Blob 存储 API 和 Azure Data Lake Gen2 API 不能互操作。

在这些 API 可互操作之前，如果需要使用同一工具来处理上传到帐户的所有内容，请不要在 Blob 存储帐户中启用分层命名空间。 使用未启用分层命名空间的存储帐户意味着无法访问 Data Lake Storage Gen2 特定的功能，例如目录和文件系统访问控制列表。

## <a name="blob-storage-apis"></a>Blob 存储 API

Blob 存储 API 在 Azure Data Lake Storage Gen2 帐户中尚不可用。

这些 API 已禁用，以防止意外出现的数据访问问题，因为 Blob 存储 API 与 Azure Data Lake Gen2 API 目前不可互操作。

如果在这些 API 被禁用之前使用了它们来加载数据，并且生产环境需要访问该数据，请联系 Azure 支持部门并提供以下信息：

* 订阅 ID（GUID，不是名称）

* 存储帐户名称

* 在生产环境中是否频繁受到影响，如果是，针对的是哪些存储帐户？

* 即使在生产环境中未频繁受到影响，也请告诉我们你是否出于某种原因需要将该数据复制到另一个存储帐户，如果是，为什么？

在这些情况下，我们可以在有限的时间段内恢复对 Blob API 的访问权限，以便你可以将此数据复制到未启用分层命名空间的存储帐户。

非托管虚拟机 (VM) 磁盘依赖于被禁用的 Blob 存储 API，因此，若要在存储帐户中启用分层命名空间，请考虑将非托管 VM 磁盘放到未启用分层命名空间的存储帐户中。

## <a name="azure-storage-explorer"></a>Azure 存储资源管理器

若要使用 Azure 存储资源管理器查看或管理 Data Lake Storage Gen2 帐户，你必须至少具有可[免费下载](https://azure.microsoft.com/features/storage-explorer/)的此工具的版本 `1.6.0`。

请注意，嵌入到 Azure 门户中的存储资源管理器的版本当前不支持查看或管理启用了分层命名空间的 Data Lake Storage Gen2 帐户。

## <a name="blob-viewing-tool"></a>Blob 查看工具

Azure 门户中的 Blob 查看工具仅为 Azure Data Lake Storage Gen2 提供有限的支持。

## <a name="third-party-applications"></a>第三方应用程序

第三方应用程序可能不支持 Azure Data Lake Storage Gen2。

是否支持该服务由每家第三方应用程序提供商决定。 目前，Blob 存储 API 和 Azure Data Lake Storage Gen2 API 不可用于管理相同的内容。 由于我们正在努力实现这种互操作性，有可能许多第三方工具可自动支持 Azure Data Lake Storage Gen2。

## <a name="azcopy-support"></a>AzCopy 支持

AzCopy 版本 8 不支持 Azure Data Lake Storage Gen2。

请改用 AzCopy 的最新预览版 ([AzCopy v10](/storage/common/storage-use-azcopy-v10?toc=%2fstorage%2ftables%2ftoc.json))，因为它支持 Azure Data Lake Storage Gen2 终结点。

## <a name="soft-delete-and-snapshots"></a>软删除和快照

软删除和快照不适用于 Azure Data Lake Storage Gen2 帐户。

所有版本控制功能（包括[快照](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob)和[软删除](/storage/blobs/storage-blob-soft-delete)）尚不适用于已启用分层命名空间的存储帐户。

## <a name="object-level-storage-tiers"></a>对象级存储层

对象级存储层（“热”和“冷”）尚不适用于 Azure Data Lake Storage Gen2 帐户，但适用于未启用分层空间的存储帐户。

## <a name="diagnostic-logs"></a>诊断日志

诊断日志不适用于 Azure Data Lake Storage Gen2 帐户。

若要请求诊断日志，请联系 Azure 支持人员。 请向他们提供你的帐户名，以及需要日志的时间段。