---
title: 配置 Azure Active Directory 身份验证
description: 了解如何在配置 Azure AD 后使用 Azure Active Directory 身份验证连接到 SQL 数据库、托管实例和 Azure Synapse。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: data warehouse
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto, carlrab
origin.date: 01/07/2020
ms.date: 03/16/2020
tags: azure-synapse
ms.openlocfilehash: 65eb4094d411d9b4acdfb8aa22f368388640eb33
ms.sourcegitcommit: dc862610e2169c1fce6fb0ae9eb7dd7567f86a0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79293757"
---
# <a name="configure-and-manage-azure-active-directory-authentication-with-sql"></a>使用 SQL 配置和管理 Azure Active Directory 身份验证

本文介绍如何创建和填充 Azure AD，然后通过 Azure [SQL 数据库](sql-database-technical-overview.md)、[托管实例](sql-database-managed-instance.md)和 [Azure Synapse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) 使用 Azure AD。 有关概述，请参阅 [Azure Active Directory 身份验证](sql-database-aad-authentication.md)。

> [!NOTE]
> 本文适用于 Azure SQL 服务器，同时也适用于在 Azure SQL 服务器中创建的 SQL 数据库和 Azure Synapse。 为简单起见，在提到 SQL 数据库和 Azure Synapse 时，本文统称 SQL 数据库。

> [!IMPORTANT]  
> 不支持使用 Azure Active Directory 帐户连接到 Azure VM 上运行的 SQL Server。 请改用域 Active Directory 帐户。

## <a name="create-and-populate-an-azure-ad"></a>创建并填充 Azure AD

创建 Azure AD 并对其填充用户和组。 Azure AD 可以是初始 Azure AD 托管域。 Azure AD 也可以是本地 Active Directory 域服务，该服务可以与 Azure AD 联合。

有关详细信息，请参阅[将本地标识与 Azure Active Directory 集成](../active-directory/hybrid/whatis-hybrid-identity.md)、[将自己的域名添加到 Azure AD](/active-directory/fundamentals/add-custom-domain)、[Azure 现在支持与 Windows Server Active Directory 联合](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/)、[管理 Azure AD 目录](../active-directory/fundamentals/active-directory-administer.md)、[使用 Windows PowerShell 管理 Azure AD](https://docs.microsoft.com/powershell/azure/overview) 和[混合标识所需端口和协议](../active-directory/hybrid/reference-connect-ports.md)。

## <a name="associate-or-add-an-azure-subscription-to-azure-active-directory"></a>将 Azure 订阅关联或添加到 Azure Active Directory

1. 通过将目录设为托管数据库的 Azure 订阅的一个受信任目录，将 Azure 订阅关联到 Azure Active Directory。 有关详细信息，请参阅 [Azure 订阅与 Azure AD 的关联方式](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)。

2. 在 Azure 门户中使用目录切换器切换到与域关联的订阅。

   > [!IMPORTANT]
   > 每个 Azure 订阅都与某个 Azure AD 实例存在信任关系。 这意味着，此订阅信任该目录对用户、服务和设备执行身份验证。 多个订阅可以信任同一个目录，但一个订阅只能信任一个目录。 订阅与目录之间的这种信任关系不同于订阅与 Azure 中所有其他资源（网站、数据库等）之间的关系，在后一种关系中，这些资源更像是订阅的子资源。 如果某个订阅过期，则对该订阅关联的其他那些资源的访问权限也会终止。 但是，目录将保留在 Azure 中，并且用户可以将另一个订阅与该目录相关联，并继续管理目录用户。 若要详细了解此受信任关系，请参阅[如何将 Azure 订阅关联或添加到 Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)。

## <a name="create-an-azure-ad-administrator-for-azure-sql-server"></a>为 Azure SQL Server 创建 Azure AD 管理员

每个 Azure SQL Server（托管 SQL 数据库或 Azure Synapse）开始时只使用单个服务器管理员帐户，即整个 Azure SQL Server 的管理员。 必须创建第二个 SQL Server 管理员，这是一个 Azure AD 帐户。 此主体在 master 数据库中作为包含的数据库用户创建。 作为管理员，服务器管理员帐户是每个用户数据库中 **db_owner** 角色的成员，并且以 **dbo** 用户身份输入每个用户数据库。 有关服务器管理员帐户的详细信息，请参阅[在 Azure SQL 数据库中管理数据库和登录名](sql-database-manage-logins.md)。

将 Azure Active Directory 与异地复制结合使用时，必须为主服务器和辅助服务器配置 Azure Active Directory 管理员。 如果服务器没有 Azure Active Directory 管理员，则 Azure Active Directory 登录名和用户会收到“无法连接到服务器”错误。

> [!NOTE]
> 如果用户使用的不是基于 Azure AD 的帐户（包括 Azure SQL Server 管理员帐户），则无法创建基于 Azure AD 的用户，这是因为他们无权使用 Azure AD 验证建议的数据库用户。

## <a name="provision-an-azure-active-directory-administrator-for-your-managed-instance"></a>为托管实例预配 Azure Active Directory 管理员

> [!IMPORTANT]
> 若要预配托管实例，则只执行以下步骤。 在 Azure AD 中，只能由全局/公司管理员或特权角色管理员执行此操作。 以下步骤介绍在目录中为具有不同权限的用户授予权限的过程。

> [!NOTE]
> 对于在正式版推出之前创建的、但正式版推出之后继续运行的 MI 的 Azure AD 管理员而言，现有的行为没有任何功能性变化。 有关更多详细信息，请参阅 [MI 的新 Azure AD 管理员功能](#new-azure-ad-admin-functionality-for-mi)。

托管实例需要权限来读取 Azure AD，以成功完成通过安全组成员资格验证用户身份或创建新用户等任务。 为此，需要授予托管实例读取 Azure AD 的权限。 有两种方法来完成此操作：从门户和 PowerShell。 以下是两种方法的步骤。

1. 在 Azure 门户右上角，选择你的连接，以下拉可能的 Active Directory 列表。

2. 选择正确的 Active Directory 作为默认的 Azure AD。

   此步骤将与 Active Directory 关联的订阅链接到托管实例，确保为 Azure AD 和托管实例使用相同的订阅。

3. 导航到托管实例，并选择想要用于 Azure AD 集成的托管实例。

   ![aad](./media/sql-database-aad-authentication/aad.png)

4. 选择“Active Directory 管理员”页顶部的横幅，并为当前用户授予权限。 如果在 Azure AD 中以全局/公司管理员身份登录，则可以通过 Azure 门户或使用 PowerShell 及以下脚本来完成。

    ![授予权限 - 门户](./media/sql-database-aad-authentication/grant-permissions.png)

    ```powershell
    # Gives Azure Active Directory read permission to a Service Principal representing the managed instance.
    # Can be executed only by a "Company Administrator", "Global Administrator", or "Privileged Role Administrator" type of user.

    $aadTenant = "<YourTenantId>" # Enter your tenant ID
    $managedInstanceName = "MyManagedInstance"

    # Get Azure AD role "Directory Users" and create if it doesn't exist
    $roleName = "Directory Readers"
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
    if ($role -eq $null) {
        # Instantiate an instance of the role template
        $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq $roleName}
        Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId
        $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
    }

    # Get service principal for managed instance
    $roleMember = Get-AzureADServicePrincipal -SearchString $managedInstanceName
    $roleMember.Count
    if ($roleMember -eq $null) {
        Write-Output "Error: No Service Principals with name '$    ($managedInstanceName)', make sure that managedInstanceName parameter was     entered correctly."
        exit
    }
    if (-not ($roleMember.Count -eq 1)) {
        Write-Output "Error: More than one service principal with name pattern '$    ($managedInstanceName)'"
        Write-Output "Dumping selected service principals...."
        $roleMember
        exit
    }

    # Check if service principal is already member of readers role
    $allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
    $selDirReader = $allDirReaders | where{$_.ObjectId -match     $roleMember.ObjectId}

    if ($selDirReader -eq $null) {
        # Add principal to readers role
        Write-Output "Adding service principal '$($managedInstanceName)' to     'Directory Readers' role'..."
        Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId     $roleMember.ObjectId
        Write-Output "'$($managedInstanceName)' service principal added to     'Directory Readers' role'..."

        #Write-Output "Dumping service principal '$($managedInstanceName)':"
        #$allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
        #$allDirReaders | where{$_.ObjectId -match $roleMember.ObjectId}
    }
    else {
        Write-Output "Service principal '$($managedInstanceName)' is already     member of 'Directory Readers' role'."
    }
    ```

5. 成功完成操作后，右上角会显示以下通知：

    ![success](./media/sql-database-aad-authentication/success.png)

6. 现在即可为托管实例选择 Azure AD 管理员。 为此，请在“Active Directory 管理员”页上选择“设置管理员”  命令。

    ![设置管理员](./media/sql-database-aad-authentication/set-admin.png)

7. 在“AAD 管理员”页中，搜索某位用户，选择该用户或组作为管理员，然后选择“选择”  。

   “Active Directory 管理员”页会显示 Active Directory 的所有成员和组。 若用户或组为灰显，则无法选择，因为不支持它们作为 Azure AD 管理员。 请参阅 [Azure AD 功能和限制](sql-database-aad-authentication.md#azure-ad-features-and-limitations)中受支持的管理员列表。 基于角色的访问控制 (RBAC) 仅适用于 Azure 门户，不会传播到 SQL Server。

    ![添加 Azure Active Directory 管理员](./media/sql-database-aad-authentication/add-azure-active-directory-admin.png)

8. 在“Active Directory 管理员”页顶部，选择“保存”  。

    ![保存](./media/sql-database-aad-authentication/save.png)

    更改管理员的过程可能需要几分钟时间。 然后，新管理员将出现在“Active Directory 管理员”框中。

在为托管实例预配 Azure AD 管理员之后，即可开始使用 <a href="https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a> 语法创建 Azure AD 服务器主体（登录名）。 有关详细信息，请参阅[托管实例概述](sql-database-managed-instance.md#azure-active-directory-integration)。

> [!TIP]
> 之后如需删除管理员，请在“Active Directory 管理员”页顶部，选择“删除管理员”  ，然后选择“保存”  。

### <a name="new-azure-ad-admin-functionality-for-mi"></a>MI 的新 Azure AD 管理员功能

下表汇总了 MI 的公共预览版 Azure AD 登录管理员功能，以及正式版随附的新 Azure AD 登录功能。

| 公共预览版中 MI 的 Azure AD 登录管理员 | MI 的正式版 Azure AD 管理员功能 |
| --- | ---|
| 行为类似于 SQL 数据库的 Azure AD 管理员，可实现 Azure AD 身份验证，但 Azure AD 管理员无法在 MI 的 master 数据库中创建 Azure AD 或 SQL 登录名。 | Azure AD 管理员拥有 sysadmin 权限，可以在 MI 的 master 数据库中创建 AAD 和 SQL 登录名。 |
| 在 sys.server_principals 视图中不存在 | 在 sys.server_principals 视图中存在 |

对于在正式版推出之前创建的、但正式版推出之后仍在运行的 MI 的 Azure AD 管理员而言，最佳做法是使用 Azure 门户中的“删除管理员”和“设置管理员”选项，为相同的 Azure AD 用户或组重置 Azure AD 管理员。

### <a name="known-issues-with-the-azure-ad-login-ga-for-mi"></a>MI 的 Azure AD 登录正式版的已知问题

- 如果某个 Azure AD 登录名在使用 T-SQL 命令 `CREATE LOGIN [myaadaccount] FROM EXTERNAL PROVIDER` 创建的 MI 的 master 数据库中存在，则无法将此登录名设置为 MI 的 Azure AD 管理员。 使用 Azure 门户、PowerShell 或 CLI 命令创建 Azure AD 登录名并将其设置为 Azure AD 管理员时会遇到错误。
  - 必须先使用命令 `DROP LOGIN [myaadaccount]` 删除 master 数据库中的登录名，然后才能以 Azure AD 管理员的身份创建帐户。
  - `DROP LOGIN` 成功后，在 Azure 门户中设置 Azure AD 管理员帐户。 
  - 如果无法设置 Azure AD 管理员帐户，请检查托管实例的 master 数据库中的登录名。 使用以下命令：`SELECT * FROM sys.server_principals`
  - 设置 MI 的 Azure AD 管理员会自动在此帐户的 master 数据库中创建登录名。 删除 Azure AD 管理员会自动删除 master 数据库中的登录名。

- 不支持将单个 Azure AD 来宾用户设置为 MI 的 Azure AD 管理员。 来宾用户必须属于要设置为 Azure AD 管理员的 Azure AD 组。目前，Azure 门户边栏选项卡不会灰显其他 Azure AD 的来宾用户，使用户能够继续进行管理员设置。 将来宾用户保存为 Azure AD 管理员会导致设置失败。
  - 若要将某个来宾用户设置为 MI 的 Azure AD 管理员，请将该来宾用户包含在 Azure AD 组中，然后将此组设置为 Azure AD 管理员。

### <a name="powershell-for-sql-managed-instance"></a>用于 SQL 的 PowerShell 托管实例

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

若要运行 PowerShell cmdlet，需要已安装并运行 Azure PowerShell。 有关详细信息，请参阅 [如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。

> [!IMPORTANT]
> PowerShell Azure 资源管理器 (RM) 模块仍受 Azure SQL 数据库支持，但所有未来的开发都是针对 Az.Sql 模块的。 AzureRM 模块至少在 2020 年 12 月之前将继续接收 bug 修补程序。  Az 模块和 AzureRm 模块中的命令参数大体上是相同的。 若要详细了解其兼容性，请参阅[新 Azure PowerShell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az)。

若要预配 Azure AD 管理员，请执行以下 Azure PowerShell 命令：

- Connect-AzAccount -Environment AzureChinaCloud
- Select-AzSubscription

Cmdlet，用于预配和管理 SQL 托管实例的 Azure AD 管理员：

| Cmdlet 名称 | 说明 |
| --- | --- |
| [Set-AzSqlInstanceActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstanceactivedirectoryadministrator) |预配当前订阅中 SQL 托管实例的 Azure AD 管理员。 （必须来自当前订阅）|
| [Remove-AzSqlInstanceActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstanceactivedirectoryadministrator) |删除当前订阅中 SQL 托管实例的 Azure AD 管理员。 |
| [Get-AzSqlInstanceActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstanceactivedirectoryadministrator) |返回当前订阅中 SQL 托管实例的 Azure AD 管理员的相关信息。|

以下命令获取名为 ManagedInstance01 的托管实例的 Azure AD 管理员的相关信息，该实例与名为 ResourceGroup01 的资源组相关联。

```powershell
Get-AzSqlInstanceActiveDirectoryAdministrator -ResourceGroupName "ResourceGroup01" -InstanceName "ManagedInstance01"
```

以下命令为名为 ManagedInstance01 的托管实例预配名为 DBA 的 Azure AD 管理员组。 此服务器与资源组 ResourceGroup01 相关联。

```powershell
Set-AzSqlInstanceActiveDirectoryAdministrator -ResourceGroupName "ResourceGroup01" -InstanceName "ManagedInstance01" -DisplayName "DBAs" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353b"
```

以下命令删除名为 ManagedInstanceName01 的托管实例的 Azure AD 管理员，该实例与资源组 ResourceGroup01 相关联。

```powershell
Remove-AzSqlInstanceActiveDirectoryAdministrator -ResourceGroupName "ResourceGroup01" -InstanceName "ManagedInstanceName01" -Confirm -PassThru
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

也可通过调用以下 CLI 命令来预配 SQL 托管实例的 Azure AD 管理员：

| 命令 | 说明 |
| --- | --- |
|[az sql mi ad-admin create](https://docs.microsoft.com/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-create) |预配 SQL 托管实例的 Azure Active Directory 管理员。 （必须来自当前订阅） |
|[az sql mi ad-admin delete](https://docs.microsoft.com/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-delete) |删除 SQL 托管实例的 Azure Active Directory 管理员。 |
|[az sql mi ad-admin list](https://docs.microsoft.com/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-list) |返回当前为 SQL 托管实例配置的 Azure Active Directory 管理员的相关信息。 |
|[az sql mi ad-admin update](https://docs.microsoft.com/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-update) |更新 SQL 托管实例的 Active Directory 管理员。 |

有关 CLI 命令的详细信息，请参阅 [az sql mi](/cli/sql/mi)。 

* * *

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server"></a>为 Azure SQL 数据库服务器预配 Azure Active Directory 管理员

> [!IMPORTANT]
> 只有在预配 Azure SQL 数据库服务器或 Azure Synapse 中的 SQL 池的情况下，才执行以下步骤。

以下两个过程说明如何使用 PowerShell 在 Azure 门户中为 Azure SQL Server 预配 Azure Active Directory 管理员。

### <a name="azure-portal"></a>Azure 门户

1. 在 [Azure 门户](https://portal.azure.cn/)右上角，选择你的连接，以下拉可能的 Active Directory 列表。 选择正确的 Active Directory 作为默认的 Azure AD。 此步骤将与订阅关联的 Active Directory 链接到 Azure SQL Server，确保为 Azure AD 和 SQL Server 使用相同的订阅。 （Azure SQL Server 托管的可能是 Azure SQL 数据库或 Azure Synapse。）

    ![选择-AD][8]

2. 搜索并选择“SQL 服务器”  。

    ![搜索并选择“SQL 服务器”](media/sql-database-aad-authentication/search-for-and-select-sql-servers.png)

    >[!NOTE]
    > 在此页面上，在选择“SQL 服务器”  前，可选择名称旁边的星号标记  以收藏  该类别，并将“SQL 服务器”  添加到左侧导航栏。

3. 在“SQL Server”  页上，选择“Active Directory 管理员”  。

4. 在“Active Directory 管理员”  页中，选择“设置管理员”  。

    ![SQL Server 的“设置 Active Directory 管理员”](./media/sql-database-aad-authentication/sql-servers-set-active-directory-admin.png)  

5. 在“添加管理员”  页中，搜索某位用户，选择该用户或组作为管理员，然后选择“选择”  。 （“Active Directory 管理员”页会显示 Active Directory 的所有成员和组。 若用户或组为灰显，则无法选择，因为不支持它们作为 Azure AD 管理员。 （请参阅[将 Azure Active Directory 身份验证与使用 SQL 数据库或 Azure Synapse 进行身份验证结合使用](sql-database-aad-authentication.md)的“Azure AD 功能和限制”部分中支持的管理员列表  。）基于角色的访问控制 (RBAC) 仅适用于该门户，不会传播到 SQL Server。

    ![选择 Azure Active Directory 管理员](./media/sql-database-aad-authentication/select-azure-active-directory-admin.png)  

6. 在“Active Directory 管理员”  页顶部，选择“保存”  。

    ![保存管理员](./media/sql-database-aad-authentication/save-admin.png)

更改管理员的过程可能需要几分钟时间。 然后，新管理员出现在“Active Directory 管理员”  框中。

   > [!NOTE]
   > 设置 Azure AD 管理员时，此新的管理员名称（用户或组）不能已作为 SQL Server 身份验证用户存在于虚拟 master 数据库中。 如果存在，Azure AD 管理员设置将失败；将回滚其创建，并指示此管理员（名称）已存在。 由于这种 SQL Server 身份验证用户不是 Azure AD 的一部分，因此使用 Azure AD 身份验证连接到服务器的任何尝试都会失败。

之后如需删除管理员，请在“Active Directory 管理员”  页顶部，选择“删除管理员”  ，然后选择“保存”  。

### <a name="powershell-for-azure-sql-database-and-azure-synapse"></a>用于 Azure SQL 数据库和 Azure Synapse 的 PowerShell

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

若要运行 PowerShell cmdlet，需要已安装并运行 Azure PowerShell。 有关详细信息，请参阅 [如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。 若要预配 Azure AD 管理员，请执行以下 Azure PowerShell 命令：

- Connect-AzAccount -Environment AzureChinaCloud
- Select-AzSubscription

Cmdlet，用于预配和管理 Azure SQL 数据库和 Azure Synapse 中的 SQL 池的 Azure AD 管理员：

| Cmdlet 名称 | 说明 |
| --- | --- |
| [Set-AzSqlServerActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlserveractivedirectoryadministrator) |为 Azure SQL Server 或 Azure Synapse 预配 Azure Active Directory 管理员。 （必须来自当前订阅） |
| [Remove-AzSqlServerActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlserveractivedirectoryadministrator) |删除 Azure SQL Server 或 Azure Synapse 的 Azure Active Directory 管理员。 |
| [Get-AzSqlServerActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserveractivedirectoryadministrator) |返回有关当前为 Azure SQL Server 或 Azure Synapse 配置的 Azure Active Directory 管理员的信息。 |

使用 PowerShell 命令 get-help 查看其中每个命令的详细信息。 例如，`get-help Set-AzSqlServerActiveDirectoryAdministrator`。

以下脚本为名为 Group-23 的资源组中的 demo_server 服务器预配名为 DBA_Group 的 Azure AD 管理员组（对象 ID `40b79501-b343-44ed-9ce7-da4c8cc7353f`）    ：

```powershell
Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" -DisplayName "DBA_Group"
```

**DisplayName** 输入参数接受 Azure AD 显示名称或用户主体名称。 例如，``DisplayName="John Smith"`` 和 ``DisplayName="johns@contoso.com"``。 对于 Azure AD 组，只支持 Azure AD 显示名称。

> [!NOTE]
> Azure PowerShell 命令 ```Set-AzSqlServerActiveDirectoryAdministrator``` 不会阻止你为不受支持的用户预配 Azure AD 管理员。 可以预配不受支持的用户，但其无法连接到数据库。

以下示例使用可选的 **ObjectID**：

```powershell
Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" `
    -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> 当 **DisplayName** 不唯一时，需要使用 Azure AD **ObjectID**。 若要检索 **ObjectID** 和 **DisplayName** 值，请使用 Azure 经典门户的 Active Directory 部分，并查看用户或组的属性。

以下示例返回有关 Azure SQL Server 的当前 Azure AD 管理员的信息：

```powershell
Get-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" | Format-List
```

以下示例删除一个 Azure AD 管理员：

```powershell
Remove-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

可以通过调用以下 CLI 命令来预配 Azure AD 管理员：

| 命令 | 说明 |
| --- | --- |
|[az sql server ad-admin create](/cli/sql/server/ad-admin#az-sql-server-ad-admin-create) |为 Azure SQL Server 或 Azure Synapse 预配 Azure Active Directory 管理员。 （必须来自当前订阅。） |
|[az sql server ad-admin delete](/cli//sql/server/ad-admin#az-sql-server-ad-admin-delete) |删除 Azure SQL Server 或 Azure Synapse 的 Azure Active Directory 管理员。 |
|[az sql server ad-admin list](/cli/sql/server/ad-admin#az-sql-server-ad-admin-list) |返回有关当前为 Azure SQL Server 或 Azure Synapse 配置的 Azure Active Directory 管理员的信息。 |
|[az sql server ad-admin update](/cli/sql/server/ad-admin#az-sql-server-ad-admin-update) |为 Azure SQL Server 或 Azure Synapse 更新 Azure Active Directory 管理员。 |

有关 CLI 命令的详细信息，请参阅 [az sql server](/cli/sql/server)。

* * *

> [!NOTE]
> 也可以使用 REST API 预配 Azure Active Directory 管理员。 有关详细信息，请参阅 [Azure SQL 数据库的 Azure SQL 数据库操作的 Service Management REST API 参考和操作](https://docs.microsoft.com/rest/api/sql/)

## <a name="configure-your-client-computers"></a>配置客户端计算机

在所有客户端计算机上，如果应用程序或用户从中使用 Azure AD 标识连接到 Azure Synapse 中的 Azure SQL 数据库或 SQL 池，则必须安装以下软件：

- .NET Framework 4.6 或更高版本（在 [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx) 上提供）。
- 适用于 SQL Server 的 Azure Active Directory 身份验证库 (*ADAL.DLL*)。 下面是下载链接，用于安装包含 *ADAL.DLL* 库的最新 SSMS、ODBC 和 OLE DB 驱动程序。
    1. [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)
    1. [ODBC Driver 17 for SQL Server](https://www.microsoft.com/download/details.aspx?id=56567)
    1. [OLE DB Driver 18 for SQL Server](https://www.microsoft.com/download/details.aspx?id=56730)

可以通过以下操作来满足这些要求：

- 安装最新版 [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) 或 [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) 符合 .NET Framework 4.6 要求。
    - SSMS 安装 *ADAL.DLL* 的 x86 版本。
    - SSDT 安装 *ADAL.DLL* 的 amd64 版本。
    - [Visual Studio 下载](https://www.visualstudio.com/downloads/download-visual-studio-vs)提供的最新 Visual Studio 符合 .NET Framework 4.6 要求，但并未安装必需的 amd64 版 *ADAL.DLL*。

## <a name="create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>在映射到 Azure AD 标识的数据库中创建包含的数据库用户

> [!IMPORTANT]
> 托管实例现在支持 Azure AD 服务器主体（登录名），因此你可以从 Azure AD 用户、组或应用程序创建登录名。 可以使用 Azure AD 服务器主体（登录名）向托管实例进行身份验证，无需将数据库用户作为包含的数据库用户创建。 有关详细信息，请参阅[托管实例概述](sql-database-managed-instance.md#azure-active-directory-integration)。 有关创建 Azure AD 服务器主体（登录名）的语法，请参阅 <a href="https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a>。

Azure Active Directory 身份验证要求以包含的数据库用户的身份创建数据库用户。 基于 Azure AD 标识的包含的数据库用户是在 master 数据库中不具有登录名的数据库用户，它映射到与数据库相关联的 Azure AD 目录中的标识。 Azure AD 标识可以是单独的用户帐户，也可以是组。 有关包含的数据库用户的详细信息，请参阅[包含的数据库用户 - 使你的数据库可移植](https://msdn.microsoft.com/library/ff929188.aspx)。

> [!NOTE]
> 不能使用 Azure 门户创建数据库用户（管理员除外）。 RBAC 角色不会传播到 Azure Synapse 中的 SQL Server、SQL 数据库或 SQL 池。 Azure RBAC 角色用于管理 Azure 资源，不会应用于数据库权限。 例如， **SQL Server 参与者** 角色不会授予连接到 Azure Synapse 中的 SQL 数据库或 SQL 池的访问权限。 必须使用 Transact-SQL 语句直接在数据库中授予访问权限。

> [!WARNING]
> 不支持在 T-SQL CREATE LOGIN 和 CREATE USER 语句中将特殊字符（例如冒号 `:` 或与号 `&`）用作用户名。

若要创建基于 Azure AD 的包含的数据库用户（而不是拥有数据库的服务器管理员），请以至少具有 **ALTER ANY USER** 权限的用户身份使用 Azure AD 标识连接到数据库。 然后，使用以下 Transact-SQL 语法：

```sql
CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
```

*Azure_AD_principal_name* 可以是 Azure AD 用户的用户主体名称，也可以是 Azure AD 组的显示名称。

**示例：** 若要创建代表 Azure AD 联合或托管域用户的包含的数据库用户：

```sql
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
CREATE USER [alice@fabrikam.partner.onmschina.cn] FROM EXTERNAL PROVIDER;
```

若要创建代表 Azure AD 或联合域组的包含的数据库用户，请提供安全组的显示名称：

```sql
CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;
```

若要创建代表可使用 Azure AD 令牌连接的应用程序的包含的数据库用户：

```sql
CREATE USER [appName] FROM EXTERNAL PROVIDER;
```

> [!NOTE]
> 此命令要求 SQL 代表已登录用户访问 Azure AD（“外部提供程序”）。 有时候，会出现导致 Azure AD 将异常返回给 SQL 的情况。 在这些情况下，用户会看到 SQL 错误 33134，其中会包含特定于 AAD 的错误消息。 大多数情况下，错误会指出访问被拒绝、用户必须注册 MFA 才能访问资源，或者在第一方应用程序之间进行的访问必须通过预授权进行处理。 如果是前面的两种情况，则问题通常是由在用户的 AAD 租户中设置的条件访问策略引起的：这些策略阻止用户访问外部提供程序。 更新 CA 策略以允许访问应用程序“00000002-0000-0000-c000-000000000000”（AAD 图形 API 的应用程序 ID）应该就能解决此问题。 如果错误指出在第一方应用程序之间进行的访问必须通过预授权进行处理，则出现问题是因为用户以服务主体的身份登录。 如果改由用户执行命令，则命名应该会成功。

> [!TIP]
> 除了与 Azure 订阅关联的 Azure Active Directory 以外，无法从 Azure Active Directory 直接创建用户。 但是，可将关联的 Active Directory 中导入的用户（称为外部用户）的其他 Active Directory 成员添加到租户 Active Directory 中的 Active Directory 组。 通过创建该 AD 组的包含数据库用户，来自外部 Active Directory 的用户可以访问 SQL 数据库。

有关基于 Azure Active Directory 标识创建包含的数据库用户的详细信息，请参阅 [CREATE USER (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx)。

> [!NOTE]
> 删除 Azure SQL Server 的 Azure Active Directory 管理员会阻止所有 Azure AD 身份验证用户连接到服务器。 必要时，SQL 数据库管理员可以手动删除无法使用的 Azure AD 用户。

> [!NOTE]
> 如果收到 **Connection Timeout Expired** 消息，则可能需要将连接字符串的 `TransparentNetworkIPResolution` 参数设置为 false。 有关详细信息，请参阅 [.NET Framework 4.6.1 的连接超时问题 - TransparentNetworkIPResolution](https://blogs.msdn.microsoft.com/dataaccesstechnologies/20../../connection-timeout-issue-with-net-framework-4-6-1-transparentnetworkipresolution/)。

创建数据库用户时，该用户会收到 **CONNECT** 权限，并能够以 **PUBLIC** 角色的成员身份连接到该数据库。 最初，仅供用户使用的权限是授予 PUBLIC 角色的任何权限，或者授予其所属任何 Azure AD 组的任何权限  。 预配基于 Azure AD 的包含的数据库用户后，可以授予用户其他权限，方法与向任何其他类型的用户授予权限相同。 通常，将权限授予数据库角色，并将用户添加到角色。 有关详细信息，请参阅 [数据库引擎权限基础知识](https://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx)。 有关特殊 SQL 数据库角色的详细信息，请参阅[在 Azure SQL 数据库中管理数据库和登录名](sql-database-manage-logins.md)。
如果将联合域用户帐户作为外部用户导入到托管域，则此用户必须使用托管域标识。

> [!NOTE]
> Azure AD 用户在数据库元数据中均标记为类型 E (EXTERNAL_USER)，而组则标记为类型 X (EXTERNAL_GROUPS)。 有关详细信息，请参阅 [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx)。

## <a name="connect-to-the-user-database-or-data-warehouse-by-using-ssms-or-ssdt"></a>使用 SSMS 或 SSDT 连接到用户数据库或数据仓库  

若要确认 Azure AD 管理员已正确设置，请使用 Azure AD 管理员帐户连接到 **master** 数据库。
若要预配基于 Azure AD 的包含的数据库用户（而不是拥有数据库的服务器管理员），请使用具有数据库访问权限的 Azure AD 标识连接到数据库。

> [!IMPORTANT]
> [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) 和 Visual Studio 2015 中的 [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) 支持 Azure Active Directory 身份验证。 2016 年 8 月版 SSMS 也包括对 Active Directory 通用身份验证的支持，这样管理员就能要求用户使用手机、短信、带 PIN 码的智能卡或移动应用通知进行多重身份验证。

## <a name="using-an-azure-ad-identity-to-connect-using-ssms-or-ssdt"></a>使用 Azure AD 标识通过 SSMS 或 SSDT 进行连接

以下过程说明如何使用 SQL Server Management Studio 或 SQL Server 数据库工具连接到具有 Azure AD 标识的 SQL 数据库。

### <a name="active-directory-integrated-authentication"></a>Active Directory 集成身份验证

如果从联合域使用 Azure Active Directory 凭据登录到 Windows，则使用此方法。

1. 启动 Management Studio 或 Data Tools 后，在“连接到服务器”  （或“连接到数据库引擎”  ）对话框的“身份验证”  框中，选择“Active Directory - 集成”  。 由于会为连接提供现有凭据，因此无需密码，也无法输入密码。

    ![选择 AD 集成身份验证][11]

2. 选择“选项”  按钮，在“连接属性”  页上的“连接到数据库”  框中，键入所要连接的用户数据库的名称。 （仅对“通用且具有 MFA 连接”  选项启用“AD 域名或租户 ID”  ，其余情况它会处于灰显状态。）  

    ![选择数据库名称][13]

## <a name="active-directory-password-authentication"></a>Active Directory 密码身份验证

在使用 Azure AD 托管域与 Azure AD 主体名称连接时使用此方法。 还可以在某些情况下（例如，在进行远程工作时）将其用于联合帐户，无需访问域。

对于本机或联合身份验证 Azure AD 用户，请使用此方法通过 Azure AD 向 SQL DB/DW 进行身份验证。 本机用户是指以显式方式在 Azure AD 中创建并使用用户名和密码进行身份验证的用户，而联合用户则是指其域与 Azure AD 联合的 Windows 用户。 当用户需要使用其 Windows 凭据（例如使用远程访问），但其本地计算机未加入域时，可以使用后一方法（用户和密码）。 在这种情况下，Windows 用户可以指定其域帐户和密码，然后使用联合凭据向 SQL DB/DW 进行身份验证。

1. 启动 Management Studio 或 Data Tools 后，在“连接到服务器”  （或“连接到数据库引擎”  ）对话框的“身份验证”  框中，选择“Active Directory - 密码”  。

2. 在“用户名”框中，以 **username\@domain.com** 格式键入 Azure Active Directory 用户名。  用户名必须是来自 Azure Active Directory 的帐户或来自与 Azure Active Directory 联合的域的帐户。

3. 在“密码”框中，为 Azure Active Directory 帐户或联合域帐户键入用户密码  。

    ![选择 AD 密码身份验证][12]

4. 选择“选项”  按钮，在“连接属性”  页上的“连接到数据库”  框中，键入所要连接的用户数据库的名称。 （请参阅前一选项的图。）

## <a name="using-an-azure-ad-identity-to-connect-from-a-client-application"></a>使用 Azure AD 标识从客户端应用程序进行连接

以下过程说明如何使用 Azure AD 标识从客户端应用程序连接到 SQL 数据库。

### <a name="active-directory-integrated-authentication"></a>Active Directory 集成身份验证

若要使用集成的 Windows 身份验证，域的 Active Directory 必须与 Azure Active Directory 联合。 连接到数据库的客户端应用程序（或服务）必须运行在已使用用户的域凭据加入域的计算机上。

若要使用集成的身份验证和 Azure AD 标识连接到数据库，数据库连接字符串中的身份验证关键字必须设置为 Active Directory Integrated。 下面的 C# 代码示例使用 ADO.NET。

```csharp
string ConnectionString = @"Data Source=n9lxnyuzhv.database.chinacloudapi.cn; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

不支持使用连接字符串关键字 `Integrated Security=True` 连接到 Azure SQL 数据库。 进行 ODBC 连接时，需要删除空格，并将“Authentication”设置为“ActiveDirectoryIntegrated”。

### <a name="active-directory-password-authentication"></a>Active Directory 密码身份验证

要使用集成的身份验证和 Azure AD 标识连接到数据库，必须将“Authentication”关键字设置为“Active Directory Password”。 连接字符串必须包含“User ID/UID”和“Password/PWD”关键字和值。 下面的 C# 代码示例使用 ADO.NET。

```csharp
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.chinacloudapi.cn; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.partner.onmschina.cn; PWD=MyPassWord!";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

通过 [Azure AD 身份验证 GitHub 演示](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth)中提供的演示代码示例，了解有关 Azure AD 身份验证方法的详细信息。

## <a name="azure-ad-token"></a>Azure AD 令牌

此身份验证方法允许从 Azure Active Directory (AAD) 获取令牌，使中间层服务能够连接到 Azure Synapse 中的 Azure SQL 数据库或 SQL 池。 这样，便可以实现包含基于证书的身份验证的复杂方案。 必须完成四个基本步骤才能使用 Azure AD 令牌身份验证：

1. 在 Azure Active Directory 中注册应用程序，并获取代码的客户端 ID。
2. 创建代表应用程序的数据库用户。 （此前已在步骤 6 中完成。）
3. 在运行应用程序的客户端计算机上创建证书。
4. 为应用程序添加用作密钥的证书。

示例连接字符串：

```csharp
string ConnectionString =@"Data Source=n9lxnyuzhv.database.chinacloudapi.cn; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
conn.AccessToken = "Your JWT token"
conn.Open();
```

有关详细信息，请参阅 [SQL Server 安全性博客](https://blogs.msdn.microsoft.com/sqlsecurity/20../../token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)。 有关添加证书的详细信息，请参阅 [Azure Active Directory 中基于证书的身份验证入门](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md)。

### <a name="sqlcmd"></a>sqlcmd

以下语句使用版本 13.1 的 sqlcmd 进行连接，该版本可从 [下载中心](https://www.microsoft.com/download/details.aspx?id=53591)下载。

> [!NOTE]
> 带 `-G` 的 `sqlcmd` 命令不适用于系统标识，它需要用户主体登录名。

```cmd
sqlcmd -S Target_DB_or_DW.testsrv.database.chinacloudapi.cn  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.chinacloudapi.cn -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="next-steps"></a>后续步骤

- 有关 SQL 数据库中的访问和控制的概述，请参阅 [SQL 数据库访问和控制](sql-database-control-access.md)。
- 有关 SQL 数据库中的登录名、用户和数据库角色的概述，请参阅[登录名、用户和数据库角色](sql-database-manage-logins.md)。
- 有关数据库主体的详细信息，请参阅[主体](https://msdn.microsoft.com/library/ms181127.aspx)。
- 有关数据库角色的详细信息，请参阅[数据库角色](https://msdn.microsoft.com/library/ms189121.aspx)。
- 有关 SQL 数据库中的防火墙规则的详细信息，请参阅 [SQL 数据库防火墙规则](sql-database-firewall-configure.md)。

<!--Image references-->
[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/active-directory-integrated.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth2.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db2.png
