---
title: 向经过身份验证的用户发送推送通知（.NET 后端）
description: 了解如何向特定用户发送推送通知
services: mobile-services,notification-hubs
documentationCenter: ios
authors: krisragh
manager: dwrede
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/21/2016
wacn.date: 09/26/2016
ms.author: krisragh
---

# 向经过身份验证的用户发送推送通知
[!INCLUDE [mobile-services-selector-push-users](../../includes/mobile-services-selector-push-users.md)]

在本主题中，你将学习如何向 iOS 上已经过身份验证的用户发送推送通知。在开始本教程之前，请先完成[身份验证入门]和[推送通知入门]教程。

在本教程中，首先你会要求用户进行身份验证，再注册通知中心以发送推送通知，然后更新服务器脚本，以便只向经过身份验证的用户发送这些通知。

##<a name="register"></a>更新服务以要求对注册进行身份验证

[!INCLUDE [mobile-services-dotnet-backend-push-notifications-app-users](../../includes/mobile-services-dotnet-backend-push-notifications-app-users.md)]

##<a name="update-app"></a>更新应用以在注册之前登录

[!INCLUDE [mobile-services-ios-push-notifications-app-users-login](../../includes/mobile-services-ios-push-notifications-app-users-login.md)]

##<a name="test"></a>测试应用

[!INCLUDE [mobile-services-ios-push-notifications-app-users-test-app](../../includes/mobile-services-ios-push-notifications-app-users-test-app.md)]

<!-- Anchors. -->
[Updating the service to require authentication for registration]: #register
[Updating the app to log in before registration]: #update-app
[Testing the app]: #test
[Next Steps]: #next-steps

<!-- URLs. -->
[身份验证入门]: ./mobile-services-dotnet-backend-ios-get-started-users.md
[推送通知入门]: ./mobile-services-dotnet-backend-ios-get-started-push.md

[Mobile Services .NET How-to Conceptual Reference]: /documentation/articles/mobile-services-windows-dotnet-how-to-use-client-library/

<!---HONumber=Mooncake_0118_2016-->