---
title: 教程 - 使用 Azure PowerShell 管理 Azure 磁盘 | Azure
description: 本教程介绍如何使用 Azure PowerShel 为虚拟机创建和管理 Azure 磁盘
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 11/05/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 24245cbfbe8fe9065e8d0a20c6ed4ea2123451fb
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52675262"
---
# <a name="tutorial---manage-azure-disks-with-azure-powershell"></a>教程 - 使用 Azure PowerShell 管理 Azure 磁盘

Azure 虚拟机使用磁盘来存储 VM 操作系统、应用程序和数据。 创建 VM 时，请务必选择适用于所需工作负荷的磁盘大小和配置。 本教程介绍如何部署和管理 VM 磁盘。 学习内容：

> [!div class="checklist"]
> * OS 磁盘和临时磁盘
> * 数据磁盘数
> * 标准磁盘和高级磁盘
> * 磁盘性能
> * 附加和准备数据磁盘

## <a name="launch-local-shell"></a>启动本地 Shell

<!--Not Available [!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]-->

如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块 5.7.0 或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。 如果在本地运行 PowerShell，则还需运行 `Connect-AzureRmAccount` 以创建与 Azure 的连接。

## <a name="default-azure-disks"></a>默认 Azure 磁盘

创建 Azure 虚拟机后，将自动向此虚拟机附加两个磁盘。 

**操作系统磁盘** - 操作系统磁盘大小最大可达 4 TB，并可托管 VM 操作系统。  OS 磁盘默认分配有一个 C: 驱动器号。 已针对 OS 性能优化了 OS 磁盘的磁盘缓存配置。 OS 磁盘不得承载应用程序或数据。 对于应用程序和数据，请使用数据磁盘，详情请参见本文稍后部分。

临时磁盘- 临时磁盘使用 VM 所在的 Azure 主机上的固态驱动器。 临时磁盘具有高性能，可用于临时数据处理等操作。 但是，如果将 VM 移动到新的主机，临时磁盘上存储的数据都将被删除。 临时磁盘的大小由 [VM 大小](sizes.md)决定。 临时磁盘默认分配有一个 D: 驱动器号。

## <a name="azure-data-disks"></a>Azure 数据磁盘

可添加额外的数据磁盘，用于安装应用程序和存储数据。 在任何需要持久和灵敏数据存储的情况下，都应使用数据磁盘。 每个数据磁盘的最大容量为 4 TB。 虚拟机的大小决定可附加到 VM 的数据磁盘数。 对于每个 VM vCPU，都可以附加四个数据磁盘。 

## <a name="vm-disk-types"></a>VM 磁盘类型

Azure 提供两种类型的磁盘。

**标准磁盘** - 受 HDD 支持，可以在确保性能的同时提供经济高效的存储。 标准磁盘适用于经济高效的开发和测试工作负荷。

**高级磁盘** - 由基于 SSD 的高性能、低延迟磁盘提供支持。 完美适用于运行生产工作负荷的 VM。 高级存储支持 DS 系列、DSv2 系列和 FS 系列 VM。 高级磁盘分为五种类型（P10、P20、P30、P40、P50），磁盘大小决定了磁盘类型。 选择时，磁盘大小值舍入为下一类型。 例如，如果大小不到 128 GB，则磁盘类型为 P10；如果大小在 129 GB 到 512 GB 之间，则磁盘类型为 P20。

<!-- Not Available on GS Series -->

### <a name="premium-disk-performance"></a>高级磁盘性能

<!--Notice:  P,E,S 60,70,80 are preview-->
|高级存储磁盘类型 | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 磁盘大小（向上舍入） | 32 GiB | 64 GiB | 128 GiB | 512 GiB | 1,024 GiB (1 TiB) | 2,048 GiB (2 TiB) | 4,095 GiB (4 TiB) |
| 每个磁盘的最大 IOPS | 120 | 240 | 500 | 2,300 | 5,000 | 7,500 | 7,500 |
| 每个磁盘的吞吐量 | 25 MB/秒 | 50 MB/秒 | 100 MB/秒 | 150 MB/秒 | 200 MB/秒 | 250 MB/秒 | 250 MB/秒 |

尽管上表确定了每个磁盘的最大 IOPS，但还可通过条带化多个数据磁盘实现更高级别的性能。 例如，可向 Standard_GS5 VM 附加 64 个数据磁盘。 如果这些磁盘的大小都为 P30，则最大可实现 80,000 IOPS。 若要详细了解每个 VM 的最大 IOPS，请参阅 [VM 类型和大小](./sizes.md)。

## <a name="create-and-attach-disks"></a>创建并附加磁盘

若要完成本教程中的示例，必须现有一个虚拟机。 需要时，使用以下命令创建虚拟机。

使用 [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) 设置虚拟机上管理员帐户所需的用户名和密码：

使用 [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) 创建虚拟机。 系统将提示你输入 VM 的管理员帐户的用户名和密码。

```PowerShell
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupDisk" `
    -Name "myVM" `
    -Location "China East" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" 
```

使用 [New-AzureRmDiskConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdiskconfig) 创建初始配置。 以下示例配置大小为 128 GB 的磁盘。

```PowerShell
$diskConfig = New-AzureRmDiskConfig `
    -Location "ChinaEast" `
    -CreateOption Empty `
    -DiskSizeGB 128
```

使用 [New-AzureRmDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk) 命令创建数据磁盘。

```PowerShell
$dataDisk = New-AzureRmDisk `
    -ResourceGroupName "myResourceGroupDisk" `
    -DiskName "myDataDisk" `
    -Disk $diskConfig
```

使用 [Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) 命令获取要向其添加数据磁盘的虚拟机。

```PowerShell
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupDisk" -Name "myVM"
```

使用 [Add-AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk) 命令向虚拟机配置添加数据磁盘。

```PowerShell
$vm = Add-AzureRmVMDataDisk `
    -VM $vm `
    -Name "myDataDisk" `
    -CreateOption Attach `
    -ManagedDiskId $dataDisk.Id `
    -Lun 1
```

使用 [Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk) 命令更新虚拟机。

```PowerShell
Update-AzureRmVM -ResourceGroupName "myResourceGroupDisk" -VM $vm
```

## <a name="prepare-data-disks"></a>准备数据磁盘

将磁盘附加到虚拟机后，需要将操作系统配置为使用该磁盘。 以下示例演示如何手动配置添加到 VM 的第一个磁盘。 还可使用[自定义脚本扩展](./tutorial-automate-vm-deployment.md)自动执行此过程。

### <a name="manual-configuration"></a>手动配置

创建与虚拟机的 RDP 连接。 打开 PowerShell 并运行此脚本。

```PowerShell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="verify-the-data-disk"></a>验证数据磁盘

若要验证是否已附加数据磁盘，请查看附加 `DataDisks` 的 `StorageProfile`。

```PowerShell
$vm.StorageProfile.DataDisks
```

输出应该类似于以下示例：

```
Name            : myDataDisk
DiskSizeGB      : 128
Lun             : 1
Caching         : None
CreateOption    : Attach
SourceImage     :
VirtualHardDisk :
```

## <a name="next-steps"></a>后续步骤

本教程中介绍了以下 VM 磁盘主题：

> [!div class="checklist"]
> * OS 磁盘和临时磁盘
> * 数据磁盘数
> * 标准磁盘和高级磁盘
> * 磁盘性能
> * 附加和准备数据磁盘

转到下一教程，了解如何自动配置 VM。

> [!div class="nextstepaction"]
> [自动配置 VM](./tutorial-automate-vm-deployment.md)

<!--Update_Description: update meta properties, wording update, update link -->
