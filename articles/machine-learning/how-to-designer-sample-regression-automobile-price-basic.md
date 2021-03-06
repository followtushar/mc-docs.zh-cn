---
title: 设计器：预测汽车价格（基本）示例
titleSuffix: Azure Machine Learning
description: 构建 ML 回归模型来预测汽车的价格，无需使用 Azure 机器学习设计器编写一行代码。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: likebupt
ms.author: v-yiso
ms.reviewer: peterlu
origin.date: 02/11/2020
ms.date: 03/16/2020
ms.openlocfilehash: 4c2fa0f3138a06be60a8b6f812a37135e8eb8c50
ms.sourcegitcommit: b7fe28ec2de92b5befe61985f76c8d0216f23430
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78850598"
---
# <a name="use-regression-to-predict-car-prices-with-azure-machine-learning-designer"></a>通过 Azure 机器学习设计器使用回归来预测汽车价格

**设计器（预览版）示例 1**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

了解如何使用设计器（预览版）在不编写代码的情况下生成机器学习回归模型。

此管道训练**决策林回归量**，以根据技术特征（如品牌、型号、动力和大小）预测汽车的价格。 因为你尝试回答问题“多少？”， 这称为回归问题。 但是，你可以应用此示例中的相同基本步骤来解决任何类型的机器学习问题，不管它是回归、分类还是聚类分析，诸如此类。

训练机器学习模型的基本步骤如下：

1. 获取数据
1. 预处理数据
1. 定型模型
1. 评估模型

下面是管道在完成后的最终图形。 本文提供所有模块的基本原理，使你可以自行做出类似的决策。

![管道的图形](./media/how-to-designer-sample-regression-automobile-price-basic/overall-graph.png)

## <a name="prerequisites"></a>先决条件

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. 单击示例 1 将其打开。


## <a name="get-the-data"></a>获取数据

此示例使用来自 UCI 机器学习存储库的**汽车价格数据（原始）** 数据集。 此数据集包含 26 个列，其中包含有关汽车的信息，包括品牌、型号、价格、车辆特征（例如气缸数）、MPG 和保险风险评分。 此示例的目标是预测汽车的价格。

## <a name="pre-process-the-data"></a>预处理数据

主要的数据准备任务包括数据清理、集成、转换、缩减、离散化或量化。 在设计器中，可以在左侧面板的“数据转换”  组中找到用于执行这些操作和其他数据预处理任务的模块。

使用**选择数据集中的列**模块排除具有许多缺失值的标准化损耗。 然后，使用**清理缺失数据**删除包含缺失值的行。 这有助于创建一组清晰的训练数据。

![数据预处理](./media/how-to-designer-sample-regression-automobile-price-basic/data-processing.png)

## <a name="train-the-model"></a>定型模型

机器学习问题各有不同。 常见的机器学习任务包括分类、聚类、回归和推荐器系统，每个系统可能需要不同的算法。 选择哪个算法通常取决于用例的要求。 选取算法后，你需要优化其参数，以训练更准确的模型。 然后，你需要基于指标（例如准确度、可理解性和效率）评估所有模型。

由于此示例的目标是预测汽车价格，并且由于标签列（价格）是连续数据，因此回归模型可能是一个不错的选择。 我们对此管道使用**线性回归**。

使用**拆分数据**模块来随机分割输入数据，使训练数据集包含 70% 的原始数据，使测试数据集包含 30% 的原始数据。

## <a name="test-evaluate-and-compare"></a>测试、评估和比较

拆分数据集并使用不同的数据集来训练和测试模型，使模型的评估更加客观。

在对模型进行训练后，可以使用**评分模型**和**评估模型**模块来生成预测结果并评估模型。

**评分模型**使用训练后的模型针对测试数据集生成预测。 若要检查结果，请选择“评分模型”的输出端口，并选择“可视化”。  

![评分结果](./media/how-to-designer-sample-regression-automobile-price-basic/sample1-score-1225.png)

将评分传递给“评估模型”模块以生成评估指标。  若要检查结果，请选择“评估模型”的输出端口，然后选择“可视化”。  

![评估结果](./media/how-to-designer-sample-regression-automobile-price-basic/sample1-evaluate-1225.png)

## <a name="clean-up-resources"></a>清理资源

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>后续步骤

浏览可用于设计器的其他示例：

- [示例 2 - 回归：比较汽车价格预测的算法](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [示例 3 - 通过特征选择进行分类：收入预测](how-to-designer-sample-classification-predict-income.md)
- [示例 4 - 分类：预测信用风险（代价敏感）](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [示例 5 - 分类：预测流失率](how-to-designer-sample-classification-churn.md)
- [示例 6 - 分类：预测航班延误](how-to-designer-sample-classification-flight-delay.md)
- [示例 7 - 文本分类：维基百科 SP 500 数据集](how-to-designer-sample-text-classification.md)