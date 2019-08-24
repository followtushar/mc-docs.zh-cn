---
title: 适用于 Windows 的 Azure AD 自助式密码重置 - Azure Active Directory
description: 如何使用 Windows 登录屏幕上的“忘记了密码”启用自助式密码重置
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
origin.date: 07/11/2019
ms.date: 08/15/2019
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: d617e9cfe0938511563003818a252e141b567adb
ms.sourcegitcommit: 8aafc2af4f15907358c02bde82bc6fab8eb2442a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/15/2019
ms.locfileid: "69448484"
---
# <a name="how-to-enable-password-reset-from-the-windows-login-screen"></a>如何：从 Windows 登录屏幕启用密码重置

对于运行 Windows 7、8、8.1、10 的计算机，可以在 Windows 登录屏幕上允许用户重置其密码。 用户不再需要查找带 Web 浏览器的设备来访问 [SSPR 门户](https://passwordreset.activedirectory.windowsazure.cn)。

![Windows 7 和 10 登录屏幕示例，其中显示了 SSPR 链接](./media/howto-sspr-windows/windows-reset-password.png)

## <a name="general-prerequisites"></a>常规先决条件

- 管理员必须通过 Azure 门户启用 Azure AD 自助式密码重置。
- **用户必须在使用此功能之前注册 SSPR**
- 网络代理要求
   - Windows 10 设备 
       - 连接到 `passwordreset.activedirectory.windowsazure.cn` 和 `ajax.aspnetcdn.com` 的端口 443
       - Windows 10 设备仅支持计算机级别的代理配置
   - Windows 7、8 和 8.1 设备
       - 连接到 `passwordreset.activedirectory.windowsazure.cn` 的端口 443

## <a name="general-limitations"></a>一般限制

- 目前不支持从远程桌面或从 Hyper-V 增强的会话进行密码重置。
- 不支持帐户解锁、移动应用通知和移动应用代码。
- 此功能不适用于部署了 802.1x 网络身份验证的网络和“在用户登录前立即执行”选项。 对于部署了 802.1x 网络身份验证的网络，建议使用计算机身份验证来启用此功能。

## <a name="windows-10-password-reset"></a>Windows 10 密码重置

### <a name="windows-10-specific-prerequisites"></a>特定于 Windows 10 的先决条件

- 至少运行 Windows 10 2018 年 4 月更新版 (v1803)，且设备必须符合下述条件之一：
    - 已加入 Azure AD
    - 已加入混合 Azure AD
- 若要使用新密码并更新缓存的凭据，已加入混合 Azure AD 的计算机必须能够通过网络连接到域控制器。
- 如果使用映像，请确保在运行 sysprep 之前先为内置 Administrator 清除 Web 缓存，再执行 CopyProfile 步骤。 有关此步骤的更多信息，可参阅支持文章：[使用自定义默认用户配置文件时性能较差](https://support.microsoft.com/help/4056823/performance-issue-with-custom-default-user-profile)。
- 已知以下设置会干扰在 Windows 10 设备上使用和重置密码的功能
    - 在 v1809 之前的 Windows 10 版本中，如果策略要求使用 Ctrl+Alt+Del，则“重置密码”将无效。 
    - 如果锁屏通知已关闭，则“重置密码”将无效。 
    - HideFastUserSwitching 设置为“启用”或 1
    - DontDisplayLastUserName 设置为“启用”或 1
    - NoLockScreen 设置为“启用”或 1
    - 在设备上设置 EnableLostMode
    - 将 Explorer.exe 替换为自定义 shell
- 组合使用下面三个特定的设置可能会导致此功能失效。
    - 交互式登录：不需要 CTRL+ALT+DEL = Disabled
    - DisableLockScreenAppNotifications = 1 或 Enabled
    - IsContentDeliveryPolicyEnforced = 1 或 True 

### <a name="enable-for-windows-10-using-the-registry"></a>为使用注册表的 Windows 10 启用此功能

1. 使用管理凭据登录到 Windows 电脑
1. 以管理员身份运行 **regedit**
1. 设置以下注册表项
   - `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      - `"AllowPasswordReset"=dword:00000001`


#### <a name="troubleshooting-windows-10-password-reset"></a>排查 Windows 10 密码重置问题

Azure AD 审核日志将包含有关密码重置发生的 IP 地址和 ClientType 的信息。

![Azure AD 审核日志中的 Windows 7 密码重置示例](./media/howto-sspr-windows/windows-7-sspr-azure-ad-audit-log.png)

当用户在 Windows 10 设备的登录屏幕中重置其密码时，系统会创建名为 `defaultuser1` 的权限较低的临时帐户。 使用此帐户可以确保密码重置过程的安全。 帐户本身有一个随机生成的密码，该密码在进行设备登录时不显示，在用户重置其密码后会由系统自动删除。 可能存在多个 `defaultuser` 配置文件，不过可以放心地忽略它们。

## <a name="windows-7-8-and-81-password-reset"></a>Windows 7、8、8.1 密码重置

### <a name="windows-7-8-and-81-specific-prerequisites"></a>特定于 Windows 7、8、8.1 的先决条件

- 修补的 Windows 7 或 Windows 8.1 操作系统。
- 根据[传输层安全性 (TLS) 注册表设置](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12)中的指导启用的 TLS 1.2。
- 如果在计算机上启用了多个第三方凭据提供程序，用户会在登录屏幕上看到多个用户配置文件。

> [!WARNING]
> 必须启用 TLS 1.2，而不能仅仅将其设置为自动协商

### <a name="install"></a>安装

1. 下载要启用的 Windows 版本的相应安装程序。
   - Microsoft 下载中心 ([https://www.microsoft.com/en-us/download/details.aspx?id=57343](https://www.microsoft.com/en-us/download/details.aspx?id=57343)) 提供了软件
1. 登录到要在其中进行安装的计算机，然后运行安装程序。
1. 安装完成后，强烈建议重新启动。
1. 重启后，在登录屏幕中选择一个用户，然后单击“忘记了密码?” 启动密码重置工作流。
1. 遵循屏幕上的步骤完成重置密码的工作流。

![在 Windows 7 中单击“忘记了密码?”后的 SSPR 流](./media/howto-sspr-windows/windows-7-sspr.png)

#### <a name="silent-installation"></a>无提示安装

- 若要进行无提示安装，请使用命令“msiexec /i SsprWindowsLogon.PROD.msi /qn”
- 若要进行无提示卸载，请使用命令“msiexec /x SsprWindowsLogon.PROD.msi /qn”

#### <a name="troubleshooting-windows-7-8-and-81-password-reset"></a>排查 Windows 7、8、8.1 的密码重置问题

事件将同时记录在计算机和 Azure AD 中。 Azure AD 事件包括有关发生密码重置的 IP 地址和 ClientType 的信息。

![Azure AD 审核日志中的 Windows 7 密码重置示例](./media/howto-sspr-windows/windows-7-sspr-azure-ad-audit-log.png)

如果需要记录更多的日志，可以更改计算机上的注册表项，以启用详细日志记录。 请仅出于故障排除的目的启用详细日志记录。

`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{86D2F0AC-2171-46CF-9998-4E33B3D7FD4F}`

- 若要启用详细日志记录，请创建 `REG_DWORD: “EnableLogging”` 并将其设置为 1。
- 若要禁用详细日志记录，请将 `REG_DWORD: “EnableLogging”` 更改为 0。

## <a name="what-do-users-see"></a>用户看到什么

为 Windows 设备配置密码重置以后，对用户来说有什么变化？ 用户如何知道可以在登录屏幕上重置其密码？

![Windows 7 和 10 登录屏幕示例，其中显示了 SSPR 链接](./media/howto-sspr-windows/windows-reset-password.png)

现在，用户在尝试登录时，可以看到“重置密码”或“忘记了密码”链接，该链接用于在登录屏幕上打开自助式密码重置体验。   此功能允许用户重置其密码，不需使用其他设备来访问 Web 浏览器。

用户可以在[重置工作或学校密码](../user-help/active-directory-passwords-update-your-own-password.md#reset-password-at-sign-in)中发现此功能的使用指南
