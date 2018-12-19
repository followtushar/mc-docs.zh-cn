---
title: 将 Robomongo 用于 Azure Cosmos DB | Azure
description: '了解如何将 Robomongo 用于 Azure Cosmos DB: API for MongoDB 帐户'
keywords: robomongo
services: cosmos-db
author: rockboyfor
manager: digimobile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: conceptual
origin.date: 05/23/2017
ms.date: 07/02/2018
ms.author: v-yeche
ms.openlocfilehash: 4ff1e9f8e865748eaf0e3639d98eedf6e0e720ca
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52655238"
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>配合使用 Robomongo 与 Azure Cosmos DB: API for MongoDB 帐户
若要使用 Robomongo 连接到 Azure Cosmos DB: API for MongoDB 帐户，必须：

* 下载并安装 [Robomongo](https://robomongo.org/)
* 具有 Azure Cosmos DB: API for MongoDB 帐户的[连接字符串](connect-mongodb-account.md)信息

## <a name="connect-using-robomongo"></a>使用 Robomongo 进行连接
要将 Azure Cosmos DB: API for MongoDB 帐户添加到 Robomongo MongoDB 连接，请执行以下步骤。

1. 使用[此处](connect-mongodb-account.md)的指令检索 Azure Cosmos DB: API for MongoDB 帐户连接信息。

    ![连接字符串边栏选项卡的屏幕截图](./media/mongodb-robomongo/connectionstringblade.png)
2. 运行 *Robomongo.exe*

3. 单击“文件”下的“连接”按钮以管理连接。 然后，在“MongoDB 连接”窗口中单击“创建”，这会打开“连接设置”窗口。

4. 在“连接设置”窗口中，选择名称。 然后，从步骤 1 的连接信息中找到**主机**和**端口**，并将其分别输入到“地址”和“端口”中。

    ![Robomongo 管理连接的屏幕截图](./media/mongodb-robomongo/manageconnections.png)
5. 在“身份验证”选项卡上，单击“执行身份验证”。 然后，输入数据库（默认值为 Admin）、**用户名**和**密码**。
**用户名**和**密码**可以在步骤 1 的连接信息中找到。

    ![Robomongo 身份验证选项卡的屏幕截图](./media/mongodb-robomongo/authentication.png)
6. 在“SSL”选项卡上，选中“使用 SSL 协议”，并将“身份验证方法”更改为“自签名证书”。

    ![Robomongo SSL 选项卡的屏幕截图](./media/mongodb-robomongo/SSL.png)
7. 最后，单击“测试”验证是否能够连接，并单击“保存”。

## <a name="next-steps"></a>后续步骤
* 浏览 Azure Cosmos DB: API for MongoDB [示例](mongodb-samples.md)。
<!-- Update_Description: update meta properties -->