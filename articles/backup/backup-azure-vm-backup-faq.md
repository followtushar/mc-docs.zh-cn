---
title: 常见问题解答 - 备份 Azure VM
description: 本文解答有关使用 Azure 备份服务备份 Azure VM 的常见问题。
ms.reviewer: sogup
author: lingliw
ms.topic: conceptual
origin.date: 09/17/2019
ms.date: 09/23/2019
ms.author: v-lingwu
ms.openlocfilehash: 3f581a8cfba47957cc7a5cea1e37d5cc6ff1756f
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79290795"
---
# <a name="frequently-asked-questions-back-up-azure-vms"></a>常见问题 - 备份 Azure VM

本文解答有关使用 [Azure 备份](backup-introduction-to-azure-backup.md)服务备份 Azure VM 的常见问题。

## <a name="backup"></a>Backup

### <a name="which-vm-images-can-be-enabled-for-backup-when-i-create-them"></a>哪些 VM 映像可以在创建时启用备份功能？

创建 VM 时，可以为运行[受支持操作系统](backup-support-matrix-iaas.md#supported-backup-actions)的 VM 启用备份

### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>备份成本包含在 VM 成本内吗？

否。 备份成本独立于 VM 的成本。 详细了解 [Azure 备份定价](https://www.azure.cn/pricing/details/backup/)。
 
### <a name="which-permissions-are-required-to-enable-backup-for-a-vm"></a>为 VM 启用备份需要哪些权限？ 

如果你是 VM 参与者，便可以在 VM 上启用备份。 如果你使用的是自定义角色，则需要具有以下权限才可在 VM 上启用备份：

- Microsoft.RecoveryServices/Vaults/write
- Microsoft.RecoveryServices/Vaults/read
- Microsoft.RecoveryServices/locations/*
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write
- Microsoft.RecoveryServices/Vaults/backupPolicies/read
- Microsoft.RecoveryServices/Vaults/backupPolicies/write

如果恢复服务保管库和 VM 的资源组不同，请确保具有恢复服务保管库资源组的写入权限。  

### <a name="does-an-on-demand-backup-job-use-the-same-retention-schedule-as-scheduled-backups"></a>按需备份作业是否与计划的备份使用相同的保留计划？
否。 为按需备份作业指定保留期。 默认情况下，从门户触发时，该作业会保留 30 天。

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>我最近在一些 VM 上启用了 Azure 磁盘加密。 我的备份是否继续有效？
提供 Azure 备份访问 Key Vault 所需的权限。 请根据 [Azure 备份 PowerShell](backup-azure-vms-automation.md) 文档的“启用备份”部分中所述，在 PowerShell 中指定权限。 

### <a name="i-migrated-vm-disks-to-managed-disks-will-my-backups-continue-to-work"></a>我将 VM 磁盘迁移到了托管磁盘。 我的备份是否继续有效？
是的，备份可以顺利工作。 无需重新配置任何设置。

### <a name="why-cant-i-see-my-vm-in-the-configure-backup-wizard"></a>为何在配置备份向导中看不到我的 VM？
该向导只会列出保管库所在的同一区域中的 VM，以及不在备份的 VM。

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>我的 VM 已关闭。 按需备份或计划备份会运行吗？
是的。 计算机关闭时，将运行备份。 恢复点标记为崩溃一致。

### <a name="can-i-cancel-an-in-progress-backup-job"></a>可以取消正在进行的备份作业吗？
是的。 可以取消处于“正在创建快照”状态的备份作业。  如果作业正在从快照传输数据，则不能将它取消。

### <a name="i-enabled-lock-on-resource-group-created-by-azure-backup-service-ie-azurebackuprg_geo_number-will-my-backups-continue-to-work"></a>我在由 Azure 备份服务创建的资源组（即 `AzureBackupRG_<geo>_<number>`）上启用了锁定，我的备份是否继续有效？
如果锁定 Azure 备份服务创建的资源组，则由于还原点的最大数目限制为 18 个，备份会开始失败。

用户需去除锁定并从该资源组中清除还原点集合，才能使未来的备份成功。请[按照这些步骤](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-azure-portal)删除还原点集合。

### <a name="does-azure-backup-support-standard-ssd-managed-disks"></a>Azure 备份是否支持标准 SSD 托管磁盘？

是的，Azure 备份支持[标准 SSD 托管磁盘](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/)。

### <a name="can-we-back-up-a-vm-with-a-write-accelerator-wa-enabled-disk"></a>可使用支持写入加速器 (WA) 的磁盘备份 VM 吗？
无法在已启用 WA 的磁盘上拍摄快照。 但是，Azure 备份服务可以从备份中排除已启用 WA 的磁盘。

### <a name="i-have-a-vm-with-write-accelerator-wa-disks-and-sap-hana-installed-how-do-i-back-up"></a>我有一个安装了写入加速器 (WA) 磁盘和 SAP HANA 的 VM。 我该如何备份？
Azure 备份无法备份已启用 WA 的磁盘，但可以将其从备份中排除。 但是，备份不会提供数据库一致性，因为未备份已启用 WA 的磁盘上的信息。 如果需要备份操作系统磁盘和备份未启用 WA 的磁盘，则可以使用此配置备份磁盘。

我们正在运行 RPO 为 15 分钟的 SAP HANA 备份的个人预览版。 它以与 SQL 数据库备份类似的方式构建，并将 backInt 接口用于 SAP HANA 认证的第三方解决方案。 如果你感兴趣，请发送电子邮件至 `AskAzureBackupTeam@microsoft.com`，主题为“注册个人预览版以备份 Azure VM 中的 SAP HANA”  。

### <a name="what-is-the-maximum-delay-i-can-expect-in-backup-start-time-from-the-scheduled-backup-time-i-have-set-in-my-vm-backup-policy"></a>从我在 VM 备份策略中设置的计划备份时间开始，我可以预期的备份开始时间的最大延迟是多少？

计划备份将在计划备份时间的 2 小时内触发。 例如，如果 100 个 VM 的备份开始时间安排在凌晨 2:00，则最多到凌晨 4:00，所有 100 个 VM 都将在运行备份作业。 如果计划的备份由于停机而暂停并已恢复/重试，则备份可以在此计划的两小时窗口之外启动。

### <a name="what-is-the-minimum-allowed-retention-range-for-daily-backup-point"></a>每日备份点允许的最小保留期限是多少？

Azure 虚拟机备份策略支持的最小保留期限为 7 天，最长为 9999 天。 对少于 7 天的现有 VM 备份策略进行任何修改都需要进行更新，以满足 7 天的最小保留期限。

## <a name="restore"></a>还原

### <a name="how-do-i-decide-whether-to-restore-disks-only-or-a-full-vm"></a>如何确定是仅还原磁盘还是要还原整个 VM？

可将 VM 还原视为 Azure VM 的快速创建选项。 此选项会更改磁盘名称、磁盘使用的容器、公共 IP 地址和网络接口名称。 创建 VM 时，更改将维护唯一资源。 VM 不会添加到可用性集。

若要执行以下操作，可使用还原磁盘选项：

- 自定义创建的 VM。 例如，更改大小。
- 添加备份时不存在的配置设置。
- 控制所创建的资源的命名约定。
- 将 VM 添加到可用性集。
- 添加必须使用 PowerShell 或模板进行配置的其他任何设置。

### <a name="can-i-restore-backups-of-unmanaged-vm-disks-after-i-upgrade-to-managed-disks"></a>升级到托管磁盘后，是否可以还原非托管 VM 磁盘的备份？
是的，可以使用从非托管磁盘迁移到托管磁盘之前创建的备份。

### <a name="how-do-i-restore-a-vm-to-a-restore-point-before-the-vm-was-migrated-to-managed-disks"></a>如何将 VM 还原至将它迁移到托管磁盘之前的某个还原点？

还原过程保持不变。 如果恢复点是 VM 具有非托管磁盘的时间点，则可以[将磁盘还原为非托管磁盘](tutorial-restore-disk.md#unmanaged-disks-restore)。 如果当时 VM 具有托管磁盘，则可以[将磁盘还原为托管磁盘](tutorial-restore-disk.md#managed-disk-restore)。 然后，可以[从这些磁盘创建 VM](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk)。

[详细了解](backup-azure-vms-automation.md#restore-an-azure-vm)如何在 PowerShell 中执行此操作。

### <a name="can-i-restore-the-vm-thats-been-deleted"></a>是否可以还原已删除的 VM？

是的。 即使删除了 VM，也仍可以转到保管库中的相应备份项，然后从恢复点还原。

### <a name="how-to-restore-a-vm-to-the-same-availability-sets"></a>如何将 VM 还原到相同的可用性集？

对于托管磁盘 Azure VM，可以在还原托管磁盘时通过在模板中提供一个选项，来实现还原到可用性集。 此模板包含名为“可用性集”的输入参数。 

### <a name="how-do-we-get-faster-restore-performances"></a>我们提高还原速度？

[即时还原](backup-instant-restore-capability.md)功能有助于更快地备份和从快照即时还原。

### <a name="what-happens-when-we-change-the-key-vault-settings-for-the-encrypted-vm"></a>更改已加密 VM 的密钥保管库设置时会发生什么情况？

更改已加密 VM 的密钥保管库设置后，备份将继续使用新的详细信息集。 但是，从更改之前的恢复点还原后，必须先在密钥保管库中还原机密，然后才能从中创建 VM。 有关详细信息，请参阅[此文](/backup/backup-azure-restore-key-secret)

机密/密钥滚动更新等操作不需要此步骤，并且在还原后可以使用相同的密钥保管库。

### <a name="can-i-access-the-vm-once-restored-due-to-a-vm-having-broken-relationship-with-domain-controller"></a>在还原后我是否由于 VM 与域控制器的关系被破坏而可以访问 VM？

可以，由于 VM 与域控制器的关系被破坏，因此在还原后可以访问 VM。 有关详细信息，请参阅[此文](/backup/backup-azure-arm-restore-vms#post-restore-steps)

## <a name="manage-vm-backups"></a>管理 VM 备份

### <a name="what-happens-if-i-modify-a-backup-policy"></a>如果修改备份策略，会发生什么情况？

VM 是使用已修改策略或新策略中的计划和保留设置备份的。

- 如果延长保留期，则会根据新策略对现有的恢复点进行标记和保留。
- 如果缩短保留期，则会将恢复点标记为在下一清理作业中删除，随后会将其删除。

### <a name="how-do-i-move-a-vm-backed-up-by-azure-backup-to-a-different-resource-group"></a>如何将 Azure 备份备份的 VM 转移到不同的资源组？

1. 暂时停止备份并保留备份数据。
2. 将 VM 移到目标资源组。
3. 在相同或新的保管库中重新启用备份。

可以从执行移动操作之前创建的可用还原点还原 VM。

### <a name="is-there-a-limit-on-number-of-vms-that-can-beassociated-with-a-same-backup-policy"></a>对于可与同一备份策略关联的 VM 数是否有限制？
有，可以从门户关联到同一备份策略的 VM 数量限制为 100 个。 我们建议，如果 VM 数超过 100 个，请创建具有相同计划或不同计划的多个备份策略。
