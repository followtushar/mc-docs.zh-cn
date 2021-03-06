---
title: 数据探索和可视化工具
titleSuffix: Azure Data Science Virtual Machine
description: 数据科学虚拟机的数据浏览和可视化工具。
keywords: 数据科学工具, 数据科学虚拟机, 数据科学工具, Linux 数据科学
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: conceptual
ms.date: 10/11/2019
ms.openlocfilehash: be22ec03468d89ea87387d3c8e1f81d084a3eaa5
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79292644"
---
# <a name="data-exploration-and-visualization-tools-on-the-azure-data-science-virtual-machine"></a>Azure Data Science Virtual Machine 上的数据浏览和可视化工具

在数据科学中，关键是要了解数据。 可视化和数据浏览工具可帮助加速数据理解。 Data Science Virtual Machine (DSVM) 上提供了以下工具，使用这些工具，可以更轻松地执行关键步骤。

## <a name="apache-drill"></a>Apache Drill
|    |           |
| ------------- | ------------- |
| 它是什么？   | 大数据的开放源代码 SQL 查询引擎    |
| 支持的 DSVM 版本      | Windows、Linux  |
| 如何在 DSVM 上配置和安装它？      |  仅以嵌入模式安装在 `/dsvm/tools/drill*` 中   |
| 典型用途      |  用于在不提取、转换、加载 (ETL) 数据的情况下进行原状数据探索。 查询不同的数据源和格式，包括 CSV、JSON、关系表和 Hadoop。     |
| 如何使用和运行它      | 桌面快捷方式  <br/> [10 分钟后即可开始钻取](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| DSVM 上的相关工具      |   Rattle、Weka、SQL Server Management Studio      |

## <a name="power-bi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| 它是什么？   | 交互式数据可视化和 BI 工具    |
| 支持的 DSVM 版本      | Windows  |
| 典型用途      |  数据可视化和构建仪表板   |
| 如何使用和运行它      | 桌面快捷方式 (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)，或直接在“开始”菜单中运行  。      |
| DSVM 上的相关工具      |   Visual Studio 2019、Visual Studio Code、Juno      |

> [!NOTE]
> 需有 Microsoft Office 365 帐户才能访问 Power BI。


## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| 它是什么？   |   用于使用 R 进行数据挖掘的图形用户界面 (GUI)   |
| 支持的 DSVM 版本     | Windows、Linux     |
| 典型用途      | R 适用的常规 UI 数据挖掘工具    |
| 如何使用和运行它      | UI 工具。 在 Windows 上，打开命令提示符，运行 R，然后在 R 中运行 `rattle()`。 在 Linux 上，使用 X2Go 连接，启动终端，运行 R，然后在 R 中运行 `rattle()`。 |
| 指向示例的链接      | [Rattle](https://togaware.com/onepager/) |
| DSVM 上的相关工具      |LightGBM、Weka、Xgboost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| 它是什么？   |  适用于数据挖掘任务的机器学习算法的集合。 这些算法可以直接应用于数据集，也可以从你自己的 Java 代码中调用。 Weka 包含用于数据预处理、分类、回归、群集、关联规则和可视化的工具。 |
| 支持的 DSVM 版本     | Windows、Linux     |
| 典型用途      | 常规机器学习工具     |
| 如何使用和运行它      | 在 Windows 上，在“开始”菜单中搜索 Weka。 在 Linux 上，使用 X2Go 进行登录，然后转到“应用程序”>“开发”>“Weka”。 |
| 示例链接      | [Weka 示例](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| DSVM 上的相关工具      |LightGBM、Rattle、Xgboost   |




