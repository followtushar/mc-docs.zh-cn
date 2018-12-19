---
title: Azure VM 备份常见问题解答
description: 针对下述常见问题的解答：Azure VM 备份原理、限制以及更改策略时会发生什么情况
services: backup
author: lingliw
manager: digimobile
keywords: azure vm 备份, azure vm 还原, 备份策略
ms.service: backup
ms.topic: conceptual
origin.date: 08/16/2018
ms.date: 11/26/2018
ms.author: v-lingwu
ms.openlocfilehash: 3d9f907d4cd2ff6b0571ae4f808beff663edf892
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52674292"
---
# <a name="questions-about-the-azure-vm-backup-service"></a>有关 Azure VM 备份服务的问题
本文提供常见问题的解答，有助于快速了解 Azure VM 备份组件。 某些答案提供内含全面信息的文章的链接。 还可以在 [论坛](https://www.azure.cn/support/contact/)中发布有关 Azure 备份服务的问题。

## <a name="configure-backup"></a>配置备份
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>恢复服务保管库是否支持经典 VM 或基于 Resource Manager 的 VM？ <br/>
恢复服务保管库同时支持这两种模型。  可以将经典 VM 或资源管理器 VM 备份到恢复服务保管库。

### <a name="what-configurations-are-not-supported-by-azure-vm-backup"></a>Azure VM 备份不支持哪些配置？
请参阅[支持的操作系统](backup-azure-arm-vms-prepare.md#supported-operating-systems-for-backup)和 [VM 备份限制](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>为什么在配置备份向导中看不到我的 VM？
在配置备份向导中，Azure 备份只列出符合以下条件的 VM：
  * 尚未进行保护 - 可以验证 VM 的备份状态，方法是：转到 VM 边栏选项卡，从“设置”菜单查看备份状态。 详细了解如何[查看 VM 的备份状态](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-operations-menu)
  * 与 VM 属于同一区域

## <a name="backup"></a>Backup
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>按需备份作业是否与计划内备份遵循相同的保留计划？
否。 应为按需备份作业指定保留期。 默认情况下，从门户触发时，该作业会保留 30 天。 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>我最近在一些 VM 上启用了 Azure 磁盘加密。 我的备份是否继续有效？
需提供 Azure 备份服务访问 Key Vault 所需的权限。 可以在 PowerShell 中使用 [PowerShell](backup-azure-vms-automation.md) 文档的“启用备份”部分所述步骤提供这些权限。

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>我将 VM 的磁盘迁移到了托管磁盘。 我的备份是否继续有效？
是的，备份可以无缝工作，无需重新配置备份。 

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>我的 VM 已关闭。 按需备份或计划备份会运行吗？
是的。 即使计算机已关闭，备份也会运行，恢复点将标记为崩溃一致。 有关更多详细信息，请参阅[此文](backup-azure-vms-introduction.md#how-does-azure-back-up-virtual-machines)中的数据一致性部分

### <a name="can-i-cancel-an-in-progress-backup-job"></a>可以取消正在进行的备份作业吗？
是的。 如果该备份作业处于“拍摄快照”阶段，则可以取消。 如果此作业正在从快照传输数据，则无法取消。 

### <a name="i-enabled-resource-group-lock-on-my-backed-up-managed-disk-vms-will-my-backups-continue-to-work"></a>我在备份的托管磁盘 VM 上启用了资源组锁定。 我的备份是否继续有效？
如果用户锁定了资源组，则备份服务将无法删除较早的还原点。 由于这一原因，新备份将开始失败，因为从后端施加了最多 18 个还原点的限制。 如果 RG 锁定后备份失败并显示内部错误，请按照以下[删除还原点集合的步骤](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-azure-portal)进行操作。

### <a name="does-backup-policy-take-daylight-saving-timedst-into-account"></a>备份策略是否考虑夏令时 (DST)？
否。 请注意，本地计算机上的日期和时间会显示当地时间以及当前夏令时偏差。 因此，由于 DST，计划备份的配置时间可能与当地时间不同。

### <a name="maximum-of-how-many-data-disks-can-i-attach-to-a-vm-to-be-backed-up-by-azure-backup"></a>我可以将多少个数据磁盘附加到 Azure 备份要备份的 VM？
Azure 备份现在支持备份最多包含 16 个磁盘的虚拟机。 若要获得 16 个磁盘支持，请[升级到 Azure VM 备份堆栈 V2](backup-upgrade-to-vm-backup-stack-v2.md)。 从 2018 年 9 月 24 日开始将支持启用保护的所有 VM。

### <a name="does-azure-backup-support-standard-ssd-managed-disk"></a>Azure 备份是否支持标准 SSD 托管磁盘？
Azure 备份支持[标准 SSD 托管磁盘](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/)，该磁盘是 Azure 虚拟机的一种新型持久存储器。 [Azure VM 备份堆栈 V2](backup-upgrade-to-vm-backup-stack-v2.md) 上的托管磁盘支持它。

## <a name="restore"></a>还原
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>如何决定是进行磁盘还原，还是进行完整的 VM 还原？
可以将 Azure 的完整 VM 还原视为一种快速创建选项。 “还原 VM”选项会更改磁盘名称、这些磁盘使用的容器、公共 IP 地址和网络接口名称。 需要更改，才能保持 VM 创建期间创建的资源的唯一性。 但它不会将 VM 添加到可用性集。 

“还原磁盘”可以用于：
  * 自定义根据时间点配置创建的 VM，例如更改大小
  * 添加在备份时不存在的配置
  * 控制所要创建资源的命名约定
  * 将 VM 添加到可用性集
  * 适用于只能通过 PowerShell/声明性模板定义实现的任何其他配置

### <a name="can-i-use-backups-of-unmanaged-disk-vm-to-restore-after-i-upgrade-my-disks-to-managed-disks"></a>将磁盘升级到托管磁盘后，是否可以使用非托管磁盘 VM 的备份进行还原？
是，可以使用将磁盘从非托管迁移到托管之前创建的备份。 默认情况下，还原 VM 作业会使用非托管磁盘创建 VM。 可以使用还原磁盘功能还原磁盘，并使用它们在托管磁盘上创建 VM。 

### <a name="what-is-the-procedure-to-restore-a-vm-to-a-restore-point-taken-before-the-conversion-from-unmanaged-to-managed-disks-was-done-for-a-vm"></a>在为 VM 完成从非托管磁盘到托管磁盘的转换之前执行的将 VM 还原到还原点的过程是怎么样的？
在此方案中，默认情况下，还原 VM 作业会使用非托管磁盘创建 VM。 若要使用托管磁盘创建 VM，请执行以下操作：
1. [还原到非托管磁盘](tutorial-restore-disk.md#restore-a-vm-disk)
2. [将还原的磁盘转换为托管磁盘](tutorial-restore-disk.md#convert-the-restored-disk-to-a-managed-disk)
3. [使用托管磁盘创建 VM](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk) <br>
有关 Powershell cmdlet，请参阅[此处](backup-azure-vms-automation.md#restore-an-azure-vm)。

### <a name="can-i-restore-the-vm-if-my-vm-is-deleted"></a>如果我的 VM 被删除，是否可以还原 VM？
是的。 VM 的生命周期及其相应的备份项是不同的。 因此，即使你删除了 VM，也可以转到恢复服务保管库中的相应备份项，并使用其中一个恢复点触发还原。 

## <a name="manage-vm-backups"></a>管理 VM 备份
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>在 VM 上更改备份策略时，会发生什么情况？
在 VM 上应用新策略时，将遵循新策略的计划和保留期。 如果延长保留期，则会对现有的恢复点进行标记，按新策略要求保留它们。 如果缩短保留期，则会将其标记为在下一清理作业中删除，随后会将其删除。 

### <a name="how-can-i-move-a-vm-enrolled-in-azure-backup-between-resource-groups"></a>如何在资源组之间移动在 Azure 备份中注册的 VM？
执行以下步骤即可成功地将备份的 VM 移至目标资源组 
1. 暂时停止备份并保留备份数据
2. 将 VM 移至目标资源组
3. 使用相同的/新的保管库对其重新进行保护

用户可以从在移动操作之前创建的可用还原点进行还原。


<!--Update_Description: wording update -->