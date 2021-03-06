---
title: 设置用于将本地物理服务器灾难恢复到 Azure 的目标环境 | Azure
description: 本文介绍如何使用 Azure Site Recovery 设置目标 Azure 环境以进行物理服务器的灾难恢复。
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: conceptual
origin.date: 11/27/2018
ms.date: 01/21/2019
ms.author: v-yeche
ms.openlocfilehash: 0ec1880b878111ff2f33458f0ba5f016ce8f8647
ms.sourcegitcommit: 26957f1f0cd708f4c9e6f18890861c44eb3f8adf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2019
ms.locfileid: "54363554"
---
# <a name="prepare-target-vmware-to-azure"></a>准备目标（VMware 到 Azure）

本文介绍如何准备 Azure 环境以将运行 Windows 或 Linux 的物理服务器 (x64) 复制到 Azure。

## <a name="prerequisites"></a>先决条件

本文假设：
- 已创建恢复服务保管库来保护物理服务器。 可通过 [Azure 门户](http://portal.azure.cn "Azure 门户")创建恢复服务保管库。
- 已[设置本地环境](physical-azure-disaster-recovery.md)用于将物理服务器复制到 Azure。

## <a name="prepare-target"></a>准备目标

完成“步骤 1: 选择保护目标”和“步骤 2: 准备源”后，会转到“步骤 3: 目标”

![准备目标](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **订阅：** 从下拉菜单中，选择要向其复制物理服务器的订阅。
2. **部署模型：** 选择部署模型（“经典”或“资源管理器”）

根据选择的部署模型，系统会运行验证来确保目标订阅至少拥有一个兼容的存储帐户和虚拟网络，可供将物理服务器复制和故障转移到其中。

成功完成验证后，单击“确定”转到下一步。

如果你没有兼容的资源管理器存储帐户或虚拟网络，则可单击页面顶部的“+ 存储帐户”或“+ 网络”按钮来创建一个。

## <a name="next-steps"></a>后续步骤
[配置复制设置](vmware-azure-set-up-replication.md)。

<!--Update_Description: update meta properties -->
