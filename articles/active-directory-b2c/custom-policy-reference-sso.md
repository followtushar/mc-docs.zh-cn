---
title: 使用自定义策略管理单一登录会话
titleSuffix: Azure AD B2C
description: 了解如何使用 Azure AD B2C 中的自定义策略管理 SSO 会话。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/04/2020
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 4e57dbb027f3118e8ff5894571ef47ff05f0df9d
ms.sourcegitcommit: 1ac138a9e7dc7834b5c0b62a133ca5ce2ea80054
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2020
ms.locfileid: "78265961"
---
# <a name="single-sign-on-session-management-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 中的单一登录会话管理

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

使用 Azure Active Directory B2C (Azure AD B2C) 中的单一登录 (SSO) 会话管理，管理员可在用户已通过身份验证之后控制与用户的交互。 例如，管理员可以控制是否显示所选的标识提供者，或是否需要再次输入本地帐户详细信息。 本文介绍如何配置 Azure AD B2C SSO 的设置。

SSO 会话管理包括两个部分。 第一个部分处理用户与 Azure AD B2C 之间的直接交互，另一个部分处理用户与外部参与方之间的交互。 Azure AD B2C 不会重写或绕过外部参与方可能保留的 SSO 会话。 通过 Azure AD B2C 转到外部参与方的路由将被“记住”，因此无需重新提示用户选择其社交或企业标识提供者。 最终的 SSO 决策仍由外部参与方做出。

SSO 会话管理使用的语义，与自定义策略中其他任何技术配置文件使用的语义相同。 执行某个业务流程步骤时，会在与该步骤关联的技术配置文件中查询 `UseTechnicalProfileForSessionManagement` 引用。 如果存在该引用，则会检查引用的 SSO 会话提供程序，确定用户是否为会话参与者。 如果是，则使用 SSO 会话提供程序来重新填充会话。 同样，在完成执行某个业务流程步骤后，如果已指定 SSO 会话提供程序，则使用该提供程序将信息存储在会话中。

Azure AD B2C 定义了大量可用的 SSO 会话提供程序：

* NoopSSOSessionProvider
* DefaultSSOSessionProvider
* ExternalLoginSSOSessionProvider
* SamlSSOSessionProvider

SSO 管理类是使用技术配置文件的 `<UseTechnicalProfileForSessionManagement ReferenceId="{ID}" />` 元素指定的。

## <a name="input-claims"></a>输入声明

`InputClaims` 元素为空或不存在。

## <a name="persisted-claims"></a>持久化声明

需要返回到应用程序或在后续步骤中由前置条件使用的声明，应该存储在会话中或通过从目录中用户配置文件读取来进行扩充。 使用持久化声明可确保身份验证旅程不会在缺少声明时失败。 若要在会话中添加声明，可使用技术配置文件的 `<PersistedClaims>` 元素。 使用提供程序重新填充会话时，持久保存的声明会添加到声明包。

## <a name="output-claims"></a>输出声明

`<OutputClaims>` 用于从会话中检索声明。

## <a name="session-providers"></a>会话提供程序

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

顾名思义，此提供程序不执行任何操作。 此提供程序可用于抑制特定技术配置文件的 SSO 行为。 

```XML
<TechnicalProfile Id="SM-Noop">
  <DisplayName>Noop Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.NoopSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

此提供程序可以用于在会话中存储声明。 此提供程序通常在用于管理本地帐户的技术配置文件中引用。 

```XML
<TechnicalProfile Id="SM-AAD">
  <DisplayName>Session Mananagement Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="objectId" />
    <PersistedClaim ClaimTypeReferenceId="signInName" />
    <PersistedClaim ClaimTypeReferenceId="authenticationSource" />
    <PersistedClaim ClaimTypeReferenceId="identityProvider" />
    <PersistedClaim ClaimTypeReferenceId="newUser" />
    <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true"/>
  </OutputClaims>
</TechnicalProfile>
```

此技术配置文件管理多重身份验证会话。

```XML
<TechnicalProfile Id="SM-MFA">
  <DisplayName>Session Mananagement Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isActiveMFASession" DefaultValue="true"/>
  </OutputClaims>
</TechnicalProfile>
```

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

此提供程序用于禁止显示“选择标识提供者”屏幕。 它通常在针对外部标识提供者配置的技术配置文件中引用。

```XML
<TechnicalProfile Id="SM-SocialLogin">
  <DisplayName>Session Mananagement Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="AlwaysFetchClaimsFromProvider">true</Item>
  </Metadata>
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="AlternativeSecurityId" />
  </PersistedClaims>
</TechnicalProfile>
```

#### <a name="metadata"></a>Metadata

| 属性 | 必须 | 说明|
| --- | --- | --- |
| AlwaysFetchClaimsFromProvider | 否 | 当前未使用，可以忽略。 |

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

此提供程序用于管理信赖方应用程序或联合 SAML 标识提供者之间的 Azure AD B2C SAML 会话。 使用 SSO 提供程序存储 SAML 标识提供者会话时，`IncludeSessionIndex` 和 `RegisterServiceProviders` 必须设为 `false`。 以下 `SM-Saml-idp` 技术配置文件由 [SAML 技术配置文件](saml-technical-profile.md)使用。

```XML
<TechnicalProfile Id="SM-Saml-idp">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="IncludeSessionIndex">false</Item>
    <Item Key="RegisterServiceProviders">false</Item>
  </Metadata>
</TechnicalProfile>
```

使用提供程序存储 B2C SAML 会话时，`IncludeSessionIndex` 和 `RegisterServiceProviders` 必须设为 `true`。 需要 `SessionIndex` 和 `NameID` 才能完成 SAML 会话注销。

以下 `SM-Saml-idp` 技术配置文件由 SAML 颁发者技术配置文件使用

```XML
<TechnicalProfile Id="SM-Saml-sp">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
</TechnicalProfile>
```
#### <a name="metadata"></a>Metadata

| 属性 | 必须 | 说明|
| --- | --- | --- |
| IncludeSessionIndex | 否 | 向提供程序指出应存储会话索引。 可能的值为 `true`（默认）或 `false`。|
| RegisterServiceProviders | 否 | 指示提供程序应注册已颁发断言的所有 SAML 服务提供程序。 可能的值为 `true`（默认）或 `false`。|




