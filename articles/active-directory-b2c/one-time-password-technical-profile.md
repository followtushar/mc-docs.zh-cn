---
title: 启用一次性密码 (OTP) 验证
titleSuffix: Azure AD B2C
description: 了解如何使用 Azure AD B2C 自定义策略设置一次性密码 (OTP) 方案。
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/20/2020
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: afb40dc4e212a29190b4ef011a75d9d83a44c5af
ms.sourcegitcommit: 1bd7711964586b41ff67fd1346dad368fe7383da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77531323"
---
# <a name="define-a-one-time-password-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>在 Azure AD B2C 自定义策略中定义一次性密码技术配置文件

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) 为管理一次性密码的生成和验证提供了支持。 使用技术配置文件生成代码，并稍后验证代码。

在代码验证过程中，一次性密码技术配置文件也可能返回错误消息。 通过使用“验证技术配置文件”来设计与一次性密码的集成  。 验证技术配置文件调用一次性密码技术配置文件来验证代码。 在用户旅程继续执行之前，验证技术配置文件将验证用户提供的数据。 使用验证技术配置文件时，错误消息将显示在自断言页面上。

## <a name="protocol"></a>协议

“Protocol”  元素的“Name”  属性必须设置为 `Proprietary`。 handler 属性必须包含 Azure AD B2C 使用的协议处理程序程序集的完全限定名称  ：

```XML
Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

以下示例显示了一次性密码技术配置文件：

```XML
<TechnicalProfile Id="VerifyCode">
  <DisplayName>Validate user input verification code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="generate-code"></a>生成代码

此技术配置文件的第一种模式是生成代码。 下面是可以为此模式配置的选项。

### <a name="input-claims"></a>输入声明

InputClaims 元素包含发送到一次性密码协议提供程序所需的声明列表  。 还可将声明名称映射到下面定义的名称。

| ClaimReferenceId | 必须 | 说明 |
| --------- | -------- | ----------- |
| 标识符 | 是 | 一个标识符，用于标识需在稍后验证代码的用户。 此标识符通常用作代码传递的目标对象的标识符，例如电子邮件地址或电话号码。 |

InputClaimsTransformations 元素可以包含 InputClaimsTransformation 元素的集合，这些元素用于在将声明发送到一次性密码协议提供程序之前修改输入声明或生成新的输入声明   。

### <a name="output-claims"></a>输出声明

OutputClaims 元素包含由一次性密码协议提供程序生成的声明列表  。 还可将声明名称映射到下面定义的名称。

| ClaimReferenceId | 必须 | 说明 |
| --------- | -------- | ----------- |
| otpGenerated | 是 | 一段生成代码，由 Azure AD B2C 管理其会话。 |

**OutputClaimsTransformations** 元素可能包含用于修改输出声明或生成新输出声明的 **OutputClaimsTransformation** 元素集合。

### <a name="metadata"></a>Metadata

以下设置可用于配置代码生成和维护：

| 属性 | 必须 | 说明 |
| --------- | -------- | ----------- |
| CodeExpirationInSeconds | 否 | 代码过期之前的时间（秒）。 最小值：`60`；大值：`1200`；默认值：`600` |
| CodeLength | 否 | 代码的长度。 默认值为 `6`。 |
| CharacterSet | 否 | 代码的字符集，其格式设置为可在正则表达式中使用。 例如，`a-z0-9A-Z`。 默认值为 `0-9`。 字符集必须在指定的集中至少包含 10 个不同的字符。 |
| NumRetryAttempts | 否 | 代码被视为无效之前的验证尝试次数。 默认值为 `5`。 |
| 操作 | 是 | 要执行的操作。 可能的值：`GenerateCode` 或 `VerifyCode`。 |
| ReuseSameCode | 否 | 给定代码未过期且仍然有效时，是否应提供重复的代码而不生成新代码。 默认值为 `false`。 |

### <a name="returning-error-message"></a>返回错误消息

代码生成模式没有返回错误消息。

### <a name="example"></a>示例

下面的示例 `TechnicalProfile` 用于生成代码：

```XML
<TechnicalProfile Id="GenerateCode">
    <DisplayName>Generate Code</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="Operation">GenerateCode</Item>
        <Item Key="CodeExpirationInSeconds">600</Item>
        <Item Key="CodeLength">6</Item>
        <Item Key="CharacterSet">0-9</Item>
        <Item Key="NumRetryAttempts">5</Item>
        <Item Key="ReuseSameCode">false</Item>
    </Metadata>
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="identifier" PartnerClaimType="identifier" />
    </InputClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="otpGenerated" PartnerClaimType="otpGenerated" />
    </OutputClaims>
</TechnicalProfile>
```

## <a name="verify-code"></a>验证验证码

此技术配置文件的第二种模式是验证代码。 下面是可以为此模式配置的选项。

### <a name="input-claims"></a>输入声明

InputClaims 元素包含发送到一次性密码协议提供程序所需的声明列表  。 还可将声明名称映射到下面定义的名称。

| ClaimReferenceId | 必须 | 说明 |
| --------- | -------- | ----------- |
| 标识符 | 是 | 一个标识符，用于标识之前生成代码的用户。 此标识符通常用作代码传递的目标对象的标识符，例如电子邮件地址或电话号码。 |
| otpToVerify | 是 | 用户提供的验证码。 |

InputClaimsTransformations 元素可以包含 InputClaimsTransformation 元素的集合，这些元素用于在将声明发送到一次性密码协议提供程序之前修改输入声明或生成新的输入声明   。

### <a name="output-claims"></a>输出声明

此协议提供程序的代码验证过程中未提供任何输出声明。

**OutputClaimsTransformations** 元素可能包含用于修改输出声明或生成新输出声明的 **OutputClaimsTransformation** 元素集合。

### <a name="metadata"></a>Metadata

以下设置可用于配置代码验证失败时显示的错误消息：

| 属性 | 必须 | 说明 |
| --------- | -------- | ----------- |
| UserMessageIfSessionDoesNotExist | 否 | 代码验证会话过期后向用户显示的消息。 代码已过期，或从未为给定标识符生成代码。 |
| UserMessageIfMaxRetryAttempted | 否 | 用户尝试验证的次数超过允许的最大值时显示的消息。 |
| UserMessageIfInvalidCode | 否 | 用户提供的代码无效时显示的消息。 |

### <a name="returning-error-message"></a>返回错误消息

如[元数据](#metadata)所述，你可以针对不同的错误情况自定义向用户显示的错误消息。 可以通过为区域设置添加前缀来进一步本地化这些消息，例如：

```XML
<Item Key="en.UserMessageIfInvalidCode">Wrong code has been entered.</Item>
```

### <a name="example"></a>示例

下面的示例 `TechnicalProfile` 用于验证代码：

```XML
<TechnicalProfile Id="VerifyCode">
    <DisplayName>Verify Code</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="Operation">VerifyCode</Item>
        <Item Key="UserMessageIfInvalidCode">Wrong code has been entered.</Item>
        <Item Key="UserMessageIfSessionDoesNotExist">Code has expired.</Item>
        <Item Key="UserMessageIfMaxRetryAttempted">You've tried too many times.</Item>
    </Metadata>
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="identifier" PartnerClaimType="identifier" />
        <InputClaim ClaimTypeReferenceId="otpGenerated" PartnerClaimType="otpToVerify" />
    </InputClaims>
</TechnicalProfile>
```

<!-- Update_Description: wording update -->


