---
title: 教程 - 使用 Azure PowerShell 创建自定义角色 | Microsoft Docs
description: 开始使用 Azure PowerShell 创建自定义角色。
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
origin.date: 06/12/2018
ms.date: 09/25/2018
ms.author: v-junlch
ms.openlocfilehash: fcbb6c99d72d85808f89e55c618e14202f7bebbe
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647427"
---
# <a name="tutorial-create-a-custom-role-using-azure-powershell"></a>教程：使用 Azure PowerShell 创建自定义角色

如果[内置角色](built-in-roles.md)不能满足你的组织的特定需求，可以创建你自己的自定义角色。 对于本教程，你将使用 Azure PowerShell 创建名为 Reader Support Tickets 的自定义角色。 该自定义角色允许用户查看订阅中的所有内容，以及创建支持票证。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 创建自定义角色
> * 列出自定义角色
> * 更新自定义角色
> * 删除自定义角色

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>先决条件

要完成本教程，需要：

- 有权创建自定义角色，例如[所有者](built-in-roles.md#owner)或[用户访问管理员](built-in-roles.md#user-access-administrator)
- 在本地安装 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

## <a name="sign-in-to-azure-powershell"></a>登录到 Azure PowerShell

登录到 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps)。

## <a name="create-a-custom-role"></a>创建自定义角色

创建自定义角色的最简单方法是从内置角色着手，对其进行编辑，然后创建新角色。

1. 在 PowerShell 中，使用 [Get-AzureRmProviderOperation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermprovideroperation) 命令获取适用于 Microsoft.Support 资源提供程序的操作列表。 这有助于了解可用来创建你的权限的操作。 还可以在 [Azure 资源管理器资源提供程序操作](resource-provider-operations.md#microsoftsupport)中查看所有操作的列表。

    ```azurepowershell
    Get-AzureRMProviderOperation "Microsoft.Support/*" | FT Operation, Description -AutoSize
    ```
    
    ```Output
    Operation                              Description
    ---------                              -----------
    Microsoft.Support/register/action      Registers to Support Resource Provider
    Microsoft.Support/supportTickets/read  Gets Support Ticket details (including status, severity, contact ...
    Microsoft.Support/supportTickets/write Creates or Updates a Support Ticket. You can create a Support Tic...
    ```

1. 使用 [Get-AzureRmRoleDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermroledefinition) 命令以 JSON 格式输出 [Reader](built-in-roles.md#reader) 角色。

    ```azurepowershell
    Get-AzureRmRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\CustomRoles\ReaderSupportRole.json
    ```

1. 在编辑器中打开 **ReaderSupportRole.json** 文件。

    下面显示了 JSON 输出。 有关不同属性的信息，请参阅[自定义角色](custom-roles.md)。

    ```json
    {
        "Name":  "Reader",
        "Id":  "acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "IsCustom":  false,
        "Description":  "Lets you view everything, but not make any changes.",
        "Actions":  [
                        "*/read"
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/"
                             ]
    }
    ```
    
1. 编辑 JSON 文件来向 `Actions` 属性添加 `"Microsoft.Support/*"` 操作。 请确保在读取操作后包括一个逗号。 此操作将允许用户创建支持票证。

1. 使用 [Get-AzureRmSubscription](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermsubscription) 命令获取订阅的 ID。

    ```azurepowershell
    Get-AzureRmSubscription
    ```

1. 在 `AssignableScopes` 中，采用以下格式添加订阅 ID：`"/subscriptions/00000000-0000-0000-0000-000000000000"`

    必须添加显式的订阅 ID，否则将不允许将角色导入到订阅中。

1. 删除 `Id` 属性行并将 `IsCustom` 属性更改为 `true`。

1. 将 `Name` 和 `Description` 属性更改为 "Reader Support Tickets" 和 "View everything in the subscription and also open support tickets"。

    JSON 文件应如下所示：

    ```json
    {
        "Name":  "Reader Support Tickets",
        "IsCustom":  true,
        "Description":  "View everything in the subscription and also open support tickets.",
        "Actions":  [
                        "*/read",
                        "Microsoft.Support/*"
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/subscriptions/00000000-0000-0000-0000-000000000000"
                             ]
    }
    ```
    
1. 若要新建自定义角色，请使用 [New-AzureRmRoleDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermroledefinition) 命令，并指定 JSON 角色定义文件。

    ```azurepowershell
    New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\ReaderSupportRole.json"
    ```

    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```
        
    现在，新的自定义角色在 Azure 门户中可用，并可分配给用户、组或服务主体，就像内置角色一样。

## <a name="list-custom-roles"></a>列出自定义角色

- 若要列出所有自定义角色，请使用 [Get-AzureRmRoleDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermroledefinition) 命令。

    ```azurepowershell
    Get-AzureRmRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
    ```

    ```Output
    Name                   IsCustom
    ----                   --------
    Reader Support Tickets     True
    ```
    
    还可以在 Azure 门户中查看自定义角色。

    ![Azure 门户中导入的自定义角色屏幕截图](./media/tutorial-custom-role-powershell/custom-role-reader-support-tickets.png)

## <a name="update-a-custom-role"></a>更新自定义角色

若要更新自定义角色，可以更新 JSON 文件或使用 `PSRoleDefinition` 对象。

1. 若要更新 JSON 文件，请使用 [Get-AzureRmRoleDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermroledefinition) 命令以 JSON 格式输出自定义角色。

    ```azurepowershell
    Get-AzureRmRoleDefinition -Name "Reader Support Tickets" | ConvertTo-Json | Out-File C:\CustomRoles\ReaderSupportRole2.json
    ```

1. 在编辑器中打开该文件。

1. 在 `Actions` 中，添加用于创建和管理资源组部署 `"Microsoft.Resources/deployments/*"` 的操作。

    更新后的 JSON 文件应如下所示：

    ```json
    {
        "Name":  "Reader Support Tickets",
        "Id":  "22222222-2222-2222-2222-222222222222",
        "IsCustom":  true,
        "Description":  "View everything in the subscription and also open support tickets.",
        "Actions":  [
                        "*/read",
                        "Microsoft.Support/*",
                        "Microsoft.Resources/deployments/*"
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/subscriptions/00000000-0000-0000-0000-000000000000"
                             ]
    }
    ```
        
1. 若要更新自定义角色，请使用 [Set-AzureRmRoleDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermroledefinition) 命令并指定更新后的 JSON 文件。

    ```azurepowershell
    Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\ReaderSupportRole2.json"
    ```

    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*, Microsoft.Resources/deployments/*}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```

1. 若要使用 `PSRoleDefintion` 对象更新你的自定义角色，请首先使用 [Get-AzureRmRoleDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermroledefinition) 命令来获取该角色。

    ```azurepowershell
    $role = Get-AzureRmRoleDefinition "Reader Support Tickets"
    ```
    
1. 调用 `Add` 方法来添加用于读取诊断设置的操作。

    ```azurepowershell
    $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*/read")
    ```

1. 使用 [Set-AzureRmRoleDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermroledefinition) 更新角色。

    ```azurepowershell
    Set-AzureRmRoleDefinition -Role $role
    ```
    
    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*, Microsoft.Resources/deployments/*,
                       Microsoft.Insights/diagnosticSettings/*/read}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```
    
## <a name="delete-a-custom-role"></a>删除自定义角色

1. 使用 [Get-AzureRmRoleDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermroledefinition) 命令获取自定义角色的 ID。

    ```azurepowershell
    Get-AzureRmRoleDefinition "Reader Support Tickets"
    ```

1. 使用 [Remove-AzureRmRoleDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermroledefinition) 命令并指定角色 ID 来删除自定义角色。

    ```azurepowershell
    Remove-AzureRmRoleDefinition -Id "22222222-2222-2222-2222-222222222222"
    ```

    ```Output
    Confirm
    Are you sure you want to remove role definition with id '22222222-2222-2222-2222-222222222222'.
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    ```

1. 系统要求确认时，请键入“Y”。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 PowerShell 创建自定义角色](custom-roles-powershell.md)

<!-- Update_Description: wording update -->