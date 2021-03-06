---
title: 使用 Azure AD 应用 Linux IaaS VM 的 Azure 磁盘加密（以前的版本）
description: 本文提供有关如何为 Linux IaaS VM 启用 Azure 磁盘加密的说明。
author: rockboyfor
ms.service: security
ms.topic: article
ms.author: v-yeche
origin.date: 03/15/2019
ms.date: 11/11/2019
ms.custom: seodec18
ms.openlocfilehash: 718a4038038d02103afa6db83dba820ba8d2362e
ms.sourcegitcommit: 9597d4da8af58009f9cef148a027ccb7b32ed8cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2019
ms.locfileid: "74655436"
---
# <a name="enable-azure-disk-encryption-with-azure-ad-on-linux-vms-previous-release"></a>在 Linux VM 上使用 Azure AD 启用 Azure 磁盘加密（以前版本）

**新版本的 Azure 磁盘加密无需提供 Azure AD 应用程序参数即可启用 VM 磁盘加密。使用新版本，在执行启用加密步骤时，不再需要提供 Azure AD 凭据。所有新 VM 都必须使用新版本在没有 Azure AD 应用程序参数的情况下进行加密。若要查看使用新版本启用 VM 磁盘加密的说明，请参阅[适用于 Linux VMS 的 Azure 磁盘加密](disk-encryption-linux.md)。已使用 Azure AD 应用程序参数加密的 VM 仍受支持，应继续使用 AAD 语法进行维护。**

可启用多种磁盘加密方案，具体步骤因方案而异。 以下部分更加详细地介绍了适用于 Linux IaaS VM 的方案。 只能对具有[支持的 VM 大小和操作系统](disk-encryption-overview.md#supported-vms-and-operating-systems)的虚拟机应用磁盘加密。 还必须满足以下先决条件：

- [VM 的其他要求](disk-encryption-overview.md#supported-vms-and-operating-systems)
- [网络和组策略](disk-encryption-overview-aad.md#networking-and-group-policy)
- [加密密钥存储要求](disk-encryption-overview-aad.md#encryption-key-storage-requirements)

加密磁盘之前创建[快照](snapshot-copy-managed-disk.md)和/或备份。 备份确保在加密过程中发生任何意外故障时可以使用恢复选项。 加密之前，需要备份包含托管磁盘的 VM。 备份之后，可以通过指定 -skipVmBackup 参数，使用 Set-AzVMDiskEncryptionExtension cmdlet 来加密托管磁盘。 有关如何备份和还原已加密 VM 的详细信息，请参阅 [Azure 备份](../../backup/backup-azure-vms-encryption.md)一文。 

>[!WARNING]
> - 如果之前是使用 [Azure 磁盘加密与 Azure AD 应用](disk-encryption-overview-aad.md)选项来加密此 VM，则必须继续使用此选项来加密 VM。 无法在此加密的 VM 上使用 [Azure 磁盘加密](disk-encryption-overview.md)，因为不支持此方案，这意味着尚不支持对此加密的 VM 实施 AAD 应用程序切换操作。
> - 为确保加密机密不会跨过区域边界，Azure 磁盘加密需要将密钥保管库和 VM 共置在同一区域。 在要加密的 VM 所在的同一区域中创建并使用 Key Vault。
> - 加密 Linux OS 卷的过程可能需要几个小时。 加密 Linux OS 卷所需的时间比加密数据卷要长，这是正常的。
> - 加密 Linux OS 卷时，应将 VM 视为不可用。 我们强烈建议在加密过程中避免 SSH 登录，以避免阻止加密过程中需要访问的任何打开文件的问题。 若要查看进度，可以使用 [Get-AzVMDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiskencryptionstatus) 或 [vm encryption show](https://docs.azure.cn/cli/vm/encryption?view=azure-cli-latest#az-vm-encryption-show) 命令。 对于 30GB 操作系统卷，此过程可能需要几小时才能完成，还需要额外的时间来加密数据卷。 除非使用“encrypt format all”选项，否则数据卷加密时间将与数据卷的大小和数量成比例。 
> - 在 Linux VM 上，仅支持对数据卷禁用加密。 如果 OS 卷已加密，则不支持对数据卷或 OS 卷禁用加密。  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="bkmk_RunningLinux"></a>在现有或正在运行的 IaaS Linux VM 上启用加密

在此方案中，可以使用资源管理器模板、PowerShell cmdlet 或 CLI 命令启用加密。 

>[!IMPORTANT]
 >启用 Azure 磁盘加密之前，必须在其外部创建基于托管磁盘的 VM 实例的快照和/或备份。 可以从门户创建托管磁盘的快照，也可以使用 [Azure 备份](../../backup/backup-azure-vms-encryption.md)。 备份确保在加密过程中发生任何意外故障时可以使用恢复选项。 备份后，可以通过指定 -skipVmBackup 参数，使用 Set-AzVMDiskEncryptionExtension cmdlet 来加密托管磁盘。 在未备份基于托管磁盘的 VM 且未指定此参数的情况下，对该 VM 使用 Set-AzVMDiskEncryptionExtension 命令会失败。 
>
>加密或禁用加密可能导致 VM 重新启动。 
>

<a name="bkmk_RunningLinuxCLI"> </a>

### <a name="enable-encryption-on-an-existing-or-running-linux-vm-using-azure-cli"></a>使用 Azure CLI 在现有或正在运行的 Linux VM 上启用加密 
可通过安装并使用 [Azure CLI 2.0](https://docs.azure.cn/cli/index?view=azure-cli-latest) 命令行工具在加密的 VHD 上启用磁盘加密。 可以在本地计算机上安装 PowerShell，并在任何 PowerShell 会话中使用它。 若要在 Azure 中现有或正在运行的 IaaS Linux VM 上启用加密，请使用以下 CLI 命令：

<!--MOONCAKE: Not Available on You can use it in your browser with [Azure local Shell](../../cloud-shell/overview.md)-->

使用 [az vm encryption enable](https://docs.azure.cn/cli/vm/encryption?view=azure-cli-latest#az-vm-encryption-enable) 命令在 Azure 中运行的 IaaS 虚拟机上启用加密。

-  **使用客户端机密加密正在运行的 VM：**

    ```azurecli
    az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
    ```

- **使用 KEK 包装客户端机密，以加密正在运行的 VM：**

    ```azurecli
    az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
    ```

    >[!NOTE]
    > disk-encryption-keyvault 参数值的语法是完整的标识符字符串：/subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]<br /> <br />
    > key-encryption-key 参数值的语法是 KEK 的完整 URI，其格式为： https://[keyvault-name].vault.azure.cn/keys/[kekname]/[kek-unique-id] 

- **验证磁盘是否已加密：** 若要检查 IaaS VM 的加密状态，请使用 [az vm encryption show](https://docs.azure.cn/cli/vm/encryption?view=azure-cli-latest#az-vm-encryption-show) 命令。 

    ```azurecli
    az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
    ```

- **禁用加密：** 若要禁用加密，请使用 [az vm encryption disable](https://docs.azure.cn/cli/vm/encryption?view=azure-cli-latest#az-vm-encryption-disable) 命令。 只允许对 Linux VM 的数据卷禁用加密。

    ```azurecli
    az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type DATA
    ```

<a name="bkmk_RunningLinuxPSH"> </a> 

### <a name="enable-encryption-on-an-existing-or-running-linux-vm-using-powershell"></a>使用 PowerShell 在现有或正在运行的 Linux VM 上启用加密
使用 [Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet 在 Azure 中运行的 IaaS 虚拟机上启用加密。 在加密磁盘之前，创建[快照](snapshot-copy-managed-disk.md)和/或使用 [Azure 备份](../../backup/backup-azure-vms-encryption.md)备份 VM。 已在 PowerShell 脚本中指定 -skipVmBackup 参数以加密正在运行的 Linux VM。

- **使用客户端机密加密正在运行的 VM：** 以下脚本初始化变量并运行 Set-AzVMDiskEncryptionExtension cmdlet。 作为先决条件，应已事先创建资源组、VM、Key Vault、AAD 应用和客户端机密。 将 MyVirtualMachineResourceGroup、MyKeyVaultResourceGroup、MySecureVM、MySecureVault、My-AAD-client-ID 和 My-AAD-client-secret 替换为自己的值。 修改 -VolumeType 参数，以指定要加密哪些磁盘。

    ```azurepowershell
    $VMRGName = 'MyVirtualMachineResourceGroup';
    $KVRGname = 'MyKeyVaultResourceGroup';
    $vmName = 'MySecureVM';
    $aadClientID = 'My-AAD-client-ID';
    $aadClientSecret = 'My-AAD-client-secret';
    $KeyVaultName = 'MySecureVault';
    $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
    $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
    $KeyVaultResourceId = $KeyVault.ResourceId;
    $sequenceVersion = [Guid]::NewGuid();

    Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
    ```
- **使用 KEK 包装客户端机密，以加密正在运行的 VM：** Azure 磁盘加密允许在 Key Vault 中指定一个现有密钥，用于包装启用加密时生成的磁盘加密机密。 指定密钥加密密钥后，Azure 磁盘加密会使用该密钥包装加密机密，然后将机密写入 Key Vault。 修改 -VolumeType 参数，以指定要加密哪些磁盘。 

    ```azurepowershell
    $KVRGname = 'MyKeyVaultResourceGroup';
    $VMRGName = 'MyVirtualMachineResourceGroup';
    $aadClientID = 'My-AAD-client-ID';
    $aadClientSecret = 'My-AAD-client-secret';
    $KeyVaultName = 'MySecureVault';
    $keyEncryptionKeyName = 'MyKeyEncryptionKey';
    $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
    $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
    $KeyVaultResourceId = $KeyVault.ResourceId;
    $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
    $sequenceVersion = [Guid]::NewGuid();

    Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
    ```

    >[!NOTE]
    > disk-encryption-keyvault 参数值的语法是完整的标识符字符串：/subscriptions/[subscription-id-guid]/resourceGroups/[KVresource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]<br /> <br />
    > key-encryption-key 参数值的语法是 KEK 的完整 URI，其格式为： https://[keyvault-name].vault.azure.cn/keys/[kekname]/[kek-unique-id] 

- **验证磁盘是否已加密：** 若要检查 IaaS VM 的加密状态，请使用 [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet。 

    ```powershell 
    Get-AzVmDiskEncryptionStatus -ResourceGroupName MyVirtualMachineResourceGroup -VMName MySecureVM
    ```

- **禁用磁盘加密：** 若要禁用加密，请使用 [Disable-AzureRmVMDiskEncryption](https://docs.microsoft.com/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet。 只允许对 Linux VM 的数据卷禁用加密。

    ```powershell 
    Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
    ```

<a name="bkmk_RunningLinux"></a>

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-with-a-template"></a>使用模板在现有或正在运行的 IaaS Linux VM 上启用加密

可通过 [Resource Manager 模板](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm) 在 Azure 中为现有或正在运行的 IaaS Linux VM 启用磁盘加密。

1. 在 Azure 快速入门模板中，单击“部署到 Azure”。 

    [![“部署到 Azure”](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-encrypt-running-linux-vm%2Fazuredeploy.json)

    >[!NOTE]
    > 必须修改从 GitHub 存储库“azure-quickstart-templates”下载或参考的模板，以适应 Azure 中国云环境。 例如，替换某些终结点（将“blob.core.windows.net”替换为“blob.core.chinacloudapi.cn”，将“cloudapp.azure.com”替换为“chinacloudapp.cn”）；必要时更改某些不受支持的 VM 映像、VM 大小、SKU 以及资源提供程序的 API 版本。

    <!--URL CORRECT ON https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-encrypt-running-linux-vm%2Fazuredeploy.json-->
    
2. 选择订阅、资源组、资源组位置、参数、法律条款和协议。 单击“创建”，在现有或正在运行的 IaaS VM 上启用加密。 

下表列出了使用 Azure AD 客户端 ID 的现有或正在运行的 VM 的 Resource Manager 模板参数：

| 参数 | 说明 |
| --- | --- |
| AADClientID | 有权将机密写入 Key Vault 的 Azure AD 应用程序的客户端 ID。 |
| AADClientSecret | 有权将机密写入 Key Vault 的 Azure AD 应用程序的客户端机密。 |
| KeyVaultName | 密钥应上传到的 Key Vault 的名称。 可以使用 Azure CLI 命令 `az keyvault show --name "MySecureVault" --query KVresourceGroup` 获取该名称。 |
|  keyEncryptionKeyURL | 用于加密所生成密钥的密钥加密密钥的 URL。 如果在 UseExistingKek 下拉列表中选择“nokek”  ，则此参数为可选参数。 如果在 UseExistingKek 下拉列表中选择“kek”  ，则必须输入 _keyEncryptionKeyURL_ 值。 |
| volumeType | 要对其执行加密操作的卷的类型。 支持的有效值为 _OS_ 或 _All_（请参阅之前先决条件部分中所述的 OS 和数据磁盘支持的 Linux 分发版及其版本）。 |
| sequenceVersion | BitLocker 操作的序列版本。 每当在同一个 VM 上执行磁盘加密操作时，此版本号便会递增。 |
| vmName | 要对其执行加密操作的 VM 的名称。 |
| 通行短语 | 键入强密码作为数据加密密钥。 |

## <a name="bkmk_EFA"></a>对 Linux IaaS VM 上的数据磁盘使用 EncryptFormatAll 功能
**EncryptFormatAll** 参数可以减少加密 Linux 数据磁盘所需的时间。 满足特定条件的分区将格式化（使用其当前文件系统）。 然后，将它们重新装回到执行命令之前所在的位置。 如果想要排除某个符合条件的数据磁盘，可以在运行命令之前卸载该磁盘。

 运行此命令之后，以前装载的所有驱动器将格式化。 然后，加密层将在现已为空的驱动器的顶层启动。 选择此选项后，附加到 VM 的临时资源磁盘也会加密。 如果重置临时驱动器，该驱动器将重新格式化，并且 Azure 磁盘加密解决方案下次有机会为 VM 重新加密该驱动器。

>[!WARNING]
> 如果 VM 的数据卷上存在所需的数据，则不应使用 EncryptFormatAll。 卸载磁盘可将其从加密项中排除。 首先应该在测试 VM 上试用 EncryptFormatAll，以了解功能参数及其影响，然后再尝试在生产 VM 上使用该参数。 EncryptFormatAll 选项会格式化数据磁盘，因此磁盘上的所有数据都会丢失。 在继续之前，请验证是否已正确卸载想要排除的磁盘。 <br /><br />
>如果在更新加密设置时设置此参数，可能会导致在实际加密之前重新启动。 在这种情况下，还需要从 fstab 文件中删除不想要格式化的磁盘。 同样，在启动加密操作之前，应将想要加密并格式化的分区添加到 fstab 文件。 

### <a name="bkmk_EFACriteria"></a>EncryptFormatAll 的条件
该参数会遍历并加密满足以下**所有**条件的所有分区： 
- 不是根/OS/启动分区
- 尚未加密
- 不是 BEK 卷
- 不是 RAID 卷
- 不是 LVM 卷
- 已装载

加密组成 RAID 或 LVM 卷而不是 RAID 或 LVM 卷的磁盘。

### <a name="bkmk_EFATemplate"></a>结合模板使用 EncryptFormatAll 参数
若要使用 EncryptFormatAll 选项，请使用可加密 Linux VM 的任何现有 Azure 资源管理器模板，并更改 AzureDiskEncryption 资源的 **EncryptionOperation** 字段。

1. 例如，使用[资源管理器模板加密正在运行的 Linux IaaS VM](https://github.com/vermashi/azure-quickstart-templates/tree/encrypt-format-running-linux-vm/201-encrypt-running-linux-vm)。 
2. 在 Azure 快速入门模板中，单击“部署到 Azure”。 
3. 将 **EncryptionOperation** 从 **EnableEncryption** 更改为 **EnableEncryptionFormatAl**
4. 选择订阅、资源组、资源组位置、其他参数、法律条款和协议。 单击“创建”，在现有或正在运行的 IaaS VM 上启用加密。 

<a name="bkmk_EFAPSH"> </a>

### <a name="use-the-encryptformatall-parameter-with-a-powershell-cmdlet"></a>结合 PowerShell cmdlet 使用 EncryptFormatAll 参数
将 [Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet 与 `EncryptFormatAll` 参数配合使用。

**使用客户端机密和 EncryptFormatAll 加密正在运行的 VM：** 例如，以下脚本会初始化变量，并结合 EncryptFormatAll 参数运行 Set-AzVMDiskEncryptionExtension cmdlet。 作为先决条件，应已事先创建资源组、VM、Key Vault、AAD 应用和客户端机密。 将 MyKeyVaultResourceGroup、MyVirtualMachineResourceGroup、MySecureVM、MySecureVault、My-AAD-client-ID 和 My-AAD-client-secret 替换为自己的值。

```azurepowershell
$KVRGname = 'MyKeyVaultResourceGroup';
$VMRGName = 'MyVirtualMachineResourceGroup'; 
$aadClientID = 'My-AAD-client-ID';
$aadClientSecret = 'My-AAD-client-secret';
$KeyVaultName = 'MySecureVault';
$KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;

Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
```

### <a name="bkmk_EFALVM"></a>结合逻辑卷管理器 (LVM) 使用 EncryptFormatAll 参数 
我们建议采用 LVM-on-crypt 设置。 对于下面的所有示例，请将设备路径和装载点替换为适合用例的任何值。 可按如下所述完成此设置：

- 添加构成 VM 的数据磁盘。
- 格式化、装载这些磁盘并将其添加到 fstab 文件。

    1. 格式化新添加的磁盘。 此处使用了 Azure 生成的符号链接。 使用符号链接可避免设备名更改所造成的问题。 有关详细信息，请参阅[排查设备名问题](troubleshoot-device-names-problems.md)一文。

        `mkfs -t ext4 /dev/disk/azure/scsi1/lun0`

    2. 装载磁盘。

        `mount /dev/disk/azure/scsi1/lun0 /mnt/mountpoint`

    3. 添加到 fstab。

        `echo "/dev/disk/azure/scsi1/lun0 /mnt/mountpoint ext4 defaults,nofail 1 2" >> /etc/fstab`

    4. 结合 -EncryptFormatAll 运行 Set-AzVMDiskEncryptionExtension PowerShell cmdlet，以加密这些磁盘。
        ```powershell
        Set-AzVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl "https://mykeyvault.vault.azure.cn/" -EncryptFormatAll
        ```
    5. 在这些新磁盘的顶层设置 LVM。 请注意，VM 在完成启动后，加密的驱动器会解锁。 因此，后续的 LVM 装载必定会延迟。

## <a name="bkmk_VHDpre"></a>通过客户加密的 VHD 和加密密钥新建的 IaaS VM
在此方案中，可以通过使用 Resource Manager 模板、PowerShell cmdlet 或 CLI 命令启用加密。 以下部分详细介绍了 Resource Manager 模板和 CLI 命令。 

参考附录中的说明来准备可在 Azure 中使用的预加密映像。 创建映像后，可使用下一部分中的步骤创建加密的 Azure VM。

* [准备预加密的 Linux VHD](disk-encryption-sample-scripts.md)

>[!IMPORTANT]
 >启用 Azure 磁盘加密之前，必须在其外部创建基于托管磁盘的 VM 实例的快照和/或备份。 可以从门户创建托管磁盘的快照，也可以使用 [Azure 备份](../../backup/backup-azure-vms-encryption.md)。 备份确保在加密过程中发生任何意外故障时可以使用恢复选项。 备份后，可以通过指定 -skipVmBackup 参数，使用 Set-AzVMDiskEncryptionExtension cmdlet 来加密托管磁盘。 在未备份基于托管磁盘的 VM 且未指定此参数的情况下，对该 VM 使用 Set-AzVMDiskEncryptionExtension 命令会失败。 
>
>加密或禁用加密可能导致 VM 重新启动。 

### <a name="bkmk_VHDprePSH"></a>使用 Azure PowerShell 加密包含预加密 VHD 的 IaaS VM 
可以使用 PowerShell cmdlet [Set-AzVMOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk#examples) 在加密的 VHD 上启用磁盘加密。 以下示例显示了一些常用参数。 

```powershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.cn/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>在新添加的数据磁盘上启用加密
可以使用 [az vm disk attach](add-disk.md) 或[通过 Azure 门户](attach-disk-portal.md)添加新数据磁盘。 在加密之前，需要先装载新附加的数据磁盘。 必须请求加密数据驱动器，因为加密正在进行时，该驱动器不可用。 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>使用 Azure CLI 在新添加的磁盘上启用加密
 如果 VM 先前使用“All”进行加密，则 --volume-type 参数应保留为 All。 All 包括 OS 和数据磁盘。 如果 VM 先前使用卷类型“OS”进行加密，则应将 --volume-type 参数更改为 All，以便包含 OS 和新数据磁盘。 如果 VM 仅使用卷类型“Data”进行加密，则它可以保留为“Data”，如下所示。 添加新数据磁盘并将其附加到 VM 并不足以为加密做准备。 在启用加密之前，还必须格式化新附加的磁盘并将其正确装载在 VM 中。 在 Linux 上，磁盘必须使用[永久性块设备名称](troubleshoot-device-names-problems.md)装载在 /etc/fstab 中。  

与 Powershell 语法相反，CLI 在启用加密时不要求用户提供唯一的序列版本。 CLI 自动生成并使用自己唯一的序列版本值。

-  **使用客户端机密加密正在运行的 VM：** 

    ```azurecli
    az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
    ```

- **使用 KEK 包装客户端机密，以加密正在运行的 VM：**

    ```azurecli
    az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
    ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>使用 Azure PowerShell 在新添加的磁盘上启用加密
 使用 Powershell 加密适用于 Linux 的新磁盘时，需要指定新的序列版本。 序列版本必须唯一。 以下脚本生成序列版本的 GUID。 

-  **使用客户端机密加密正在运行的 VM：** 以下脚本初始化变量并运行 Set-AzVMDiskEncryptionExtension cmdlet。 作为先决条件，应已事先创建资源组、VM、Key Vault、AAD 应用和客户端机密。 将 MyVirtualMachineResourceGroup、MyKeyVaultResourceGroup、MySecureVM、MySecureVault、My-AAD-client-ID 和 My-AAD-client-secret 替换为自己的值。 -VolumeType 参数设置为数据磁盘而不是 OS 磁盘。 如果 VM 先前使用卷类型“OS”或“All”进行加密，则应将 -VolumeType 参数更改为 All，以便包含 OS 和新数据磁盘。

    ```azurepowershell
    $KVRGname = 'MyKeyVaultResourceGroup';
    $VMRGName = 'MyVirtualMachineResourceGroup'; 
    $vmName = 'MySecureVM';
    $aadClientID = 'My-AAD-client-ID';
    $aadClientSecret = 'My-AAD-client-secret';
    $KeyVaultName = 'MySecureVault';
    $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
    $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
    $KeyVaultResourceId = $KeyVault.ResourceId;
    $sequenceVersion = [Guid]::NewGuid();

    Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' -SequenceVersion $sequenceVersion;
    ```
- **使用 KEK 包装客户端机密，以加密正在运行的 VM：** Azure 磁盘加密允许在 Key Vault 中指定一个现有密钥，用于包装启用加密时生成的磁盘加密机密。 指定密钥加密密钥后，Azure 磁盘加密会使用该密钥包装加密机密，然后将机密写入 Key Vault。 -VolumeType 参数设置为数据磁盘而不是 OS 磁盘。 如果 VM 先前使用卷类型“OS”或“All”进行加密，则应将 -VolumeType 参数更改为 All，以便包含 OS 和新数据磁盘。

    ```azurepowershell
    $KVRGname = 'MyKeyVaultResourceGroup';
    $VMRGName = 'MyVirtualMachineResourceGroup';
    $vmName = 'MyExtraSecureVM';
    $aadClientID = 'My-AAD-client-ID';
    $aadClientSecret = 'My-AAD-client-secret';
    $KeyVaultName = 'MySecureVault';
    $keyEncryptionKeyName = 'MyKeyEncryptionKey';
    $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
    $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
    $KeyVaultResourceId = $KeyVault.ResourceId;
    $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
    $sequenceVersion = [Guid]::NewGuid();

    Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' -SequenceVersion $sequenceVersion;
    ```

    >[!NOTE]
    > disk-encryption-keyvault 参数值的语法是完整的标识符字符串：/subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] <br /> <br />
    > key-encryption-key 参数值的语法是 KEK 的完整 URI，其格式为： https://[keyvault-name].vault.azure.cn/keys/[kekname]/[kek-unique-id] 

## <a name="disable-encryption-for-linux-vms"></a>为 Linux VM 禁用加密
可以使用 Azure PowerShell、Azure CLI 或资源管理器模板禁用加密。 

>[!IMPORTANT]
>在 Linux VM 上，仅支持对数据卷禁用 Azure 磁盘加密。 如果 OS 卷已加密，则不支持对数据卷或 OS 卷禁用加密。  

- **使用 Azure PowerShell 禁用磁盘加密：** 若要禁用加密，请使用 [Disable-AzureRmVMDiskEncryption](https://docs.microsoft.com/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet。 
    ```powershell
    Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' [--volume-type {ALL, DATA, OS}]
    ```

- **使用 Azure CLI 禁用加密：** 若要禁用加密，请使用 [az vm encryption disable](https://docs.azure.cn/cli/vm/encryption?view=azure-cli-latest#az-vm-encryption-disable) 命令。 
    ```azurecli
    az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
    ```
- **使用资源管理器模板禁用加密：** 使用[在正在运行的 Linux VM 上禁用加密](https://aka.ms/decrypt-linuxvm)模板禁用加密。
    1. 单击 **“部署到 Azure”** 。
    2. 选择订阅、资源组、位置、VM、法律条款和协议。
    3. 单击“购买”，在正在运行的 Windows VM 上禁用磁盘加密。  

## <a name="next-steps"></a>后续步骤

- [适用于 Linux 的 Azure 磁盘加密概述](disk-encryption-overview-aad.md)
- [使用 Azure AD 创建和配置用于 Azure 磁盘加密的密钥保管库（以前版本）](disk-encryption-key-vault-aad.md)

<!-- Update_Description: new article about disk encryption linux aad -->
<!--NEW.date: 11/11/2019-->