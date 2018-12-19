---
title: 如何在 Azure Active Directory 中重置用户的密码 | Microsoft Docs
description: 了解如何使用 Azure Active Directory 重置用户的密码。
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
origin.date: 09/05/2018
ms.date: 10/09/2018
ms.author: v-junlch
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: b8ccc6f3b4a43f3690002268310c9ed6286fad73
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52662804"
---
# <a name="how-to-reset-a-users-password-using-azure-active-directory"></a>如何：使用 Azure Active Directory 重置用户的密码
如果忘记了密码，用户被锁定在设备外，或者用户从未收到密码，则可以重置用户的密码。

>[!Note]
>除非你的 Azure AD 租户是用户的主目录，否则你无法重置用户的密码。 这意味着，如果用户使用另一个组织中的帐户、Microsoft 帐户登录到你的组织，则你无法重置其密码。<br><br>如果用户使用外部 Azure AD 作为授权来源，则你无法重置密码。 只有用户或外部 Azure AD 中的管理员可以重置密码。

## <a name="to-reset-a-password"></a>重置密码

1. 以全局管理员、用户管理员或密码管理员身份登录到 [Azure 门户](https://portal.azure.cn/)。 有关可用角色的详细信息，请参阅[在 Azure Active Directory 中分配管理员角色](../users-groups-roles/directory-assign-admin-roles.md#available-roles)

2. 选择“Azure Active Directory”，选择“用户”，搜索并选择需要重置的用户，然后选择“重置密码”。

    此时将显示“Alain Charon - 个人资料”页面，其中包含“重置密码”选项。

    ![用户的个人资料页面，其中突出显示了“重置密码”选项](./media/active-directory-users-reset-password-azure-portal/user-profile-reset-password-link.png)

3. 在“重置密码”页面中，选择“重置密码”。

    将会自动为用户生成一个临时密码。

4. 复制该密码并将其提供给用户。 用户在下次登录过程中需要更改密码。

    >[!Note]
    >临时密码永不过期。 用户下次登录时，密码仍将有效，不管从生成临时密码后已经过去了多长时间。

## <a name="next-steps"></a>后续步骤
重置用户的密码后，可以执行以下基本流程：

- [添加或删除用户](add-users-azure-active-directory.md)

- [向用户分配角色](active-directory-users-assign-role-azure-portal.md)

- [添加或更改个人资料信息](active-directory-users-profile-azure-portal.md)

- [创建基本组并添加成员](active-directory-groups-create-azure-portal.md)

还可以执行更复杂的用户方案，例如，分配委托、使用策略，以及共享用户帐户。 有关其他可用操作的详细信息，请参阅 [Azure Active Directory 用户管理和文档](../users-groups-roles/index.yml)。

<!-- Update_Description: wording update -->