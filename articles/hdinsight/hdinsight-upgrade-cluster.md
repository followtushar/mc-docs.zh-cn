---
title: 将群集迁移到较新的版本
titleSuffix: Azure HDInsight
description: 了解将 Azure HDInsight 群集迁移到较新版本的准则。
author: hrasheed-msft
services: hdinsight
ms.service: hdinsight
ms.author: v-yiso
ms.custom: hdinsightactive
ms.topic: conceptual
origin.date: 01/31/2020
ms.date: 02/24/2020
ms.openlocfilehash: 6ccc8a4c5a6d49d680a0ab689498fa3b9d6186fe
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77428697"
---
# <a name="migrate-hdinsight-cluster-to-a-newer-version"></a>将 HDInsight 群集迁移到较新版本

若要利用最新的 HDInsight 功能，建议将 HDInsight 群集定期迁移到最新版本。 HDInsight 不支持将现有群集升级到较新组件版本的就地升级。 必须使用所需的组件和平台版本创建新群集，然后迁移应用程序以使用新群集。 按照以下准则迁移 HDInsight 群集版本。

> [!NOTE]
> 有关支持的 HDInsight 版本的信息，请参阅 [HDInsight 组件版本](hdinsight-component-versioning.md#supported-hdinsight-versions)。
>
>
## <a name="migration-tasks"></a>迁移任务

升级 HDInsight 群集的工作流如下所示。

![升级工作流关系图](./media/hdinsight-upgrade-cluster/upgrade-workflow-diagram.png)

1. 请阅读本文档的每个部分，以了解升级 Linux 群集时可能需要进行的更改。
2. 创建群集作为测试/质量保证环境。 有关创建群集的详细信息，请参阅[了解如何创建基于 Linux 的 HDInsight 群集](hdinsight-hadoop-provision-linux-clusters.md)
3. 将现有作业、数据源及接收器复制到新环境。
4. 执行验证测试，以确保作业在新群集上按预期工作。


验证一切都按预期工作后，请为迁移安排停机时间。 在此停机期间，请执行以下操作：

1.  备份所有存储在本地群集节点上的暂时性数据。 例如，如果数据直接存储在头节点上。
2.  删除现有的群集。
3.  使用以前群集使用的同一默认数据存储在与最新（或支持）的 HDI 版本相同的 VNET 子网中创建群集。 这样，新群集便可根据现有生产数据继续运行。
4.  导入任何已备份的暂时性数据。
5.  使用新群集启动作业/继续处理。

## <a name="workload-specific-guidance"></a>特定于工作负载的指南

以下文档提供有关如何迁移特定工作负载的指南：

* [迁移 HBase](/hbase/apache-hbase-migrate-new-version.md)
* [迁移 Kafka](/kafka/migrate-versions.md)
* [迁移 Hive/Interactive Query](/interactive-query/apache-hive-migrate-workloads.md)

## <a name="backup-and-restore"></a>备份和还原

有关数据库备份和还原的详细信息，请参阅[使用自动数据库备份恢复 Azure SQL 数据库](../sql-database/sql-database-recovery-using-backups.md)。

## <a name="next-steps"></a>后续步骤

* [了解如何创建基于 Linux 的 HDInsight 群集](hdinsight-hadoop-provision-linux-clusters.md)
* [使用 SSH 连接到 HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
* [使用 Apache Ambari 管理基于 Linux 的群集](hdinsight-hadoop-manage-ambari.md)
* [HDInsight 发行说明](./hdinsight-version-release.md)
