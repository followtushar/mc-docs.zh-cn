---
title: 使用 Ruby 查询 Azure SQL 数据库 | Microsoft Docs
description: 本主题介绍如何使用 Ruby 创建可连接到 Azure SQL 数据库的程序并使用 Transact-SQL 语句对其进行查询。
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ruby
ms.topic: quickstart
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: craigg
origin.date: 11/01/2018
ms.date: 12/03/2018
ms.openlocfilehash: b47a4987a10fb3ba50e6c1b66ba8ea31533cbabf
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52672999"
---
# <a name="quickstart-use-ruby-to-query-an-azure-sql-database"></a>快速入门：使用 Ruby 查询 Azure SQL 数据库

此快速入门演示如何使用 [Ruby](https://www.ruby-lang.org) 来创建连接到 Azure SQL 数据库的程序，并使用 Transact-SQL 语句来查询数据。

## <a name="prerequisites"></a>先决条件

若要完成本快速入门，请确保符合以下先决条件：

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- 针对用于本快速入门的计算机的公共 IP 地址制定[服务器级防火墙规则](sql-database-get-started-portal-firewall.md)。

- 为操作系统安装了 Ruby 和相关软件：
    - **MacOS**：依次安装 Homebrew、rbenv、ruby-build、Ruby 和 FreeTDS。 请参阅[步骤 1.2、1.3、1.4 和 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/)。
    - **Ubuntu**：依次安装 Ruby 必备组件、rbenv、ruby-build、Ruby 和 FreeTDS。 请参阅[步骤 1.2、1.3、1.4 和 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/)。

## <a name="sql-server-connection-information"></a>SQL Server 连接信息

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

> [!IMPORTANT]
> 对于在其上执行本教程操作的计算机，必须为其公共 IP 地址制定防火墙规则。 如果使用其他计算机或其他公共 IP 地址，则[使用 Azure 门户创建服务器级防火墙规则](sql-database-get-started-portal-firewall.md)。 

## <a name="insert-code-to-query-sql-database"></a>插入用于查询 SQL 数据库的代码

1. 在常用的文本编辑器中创建一个新文件 **sqltest.rb**

2. 将内容替换为以下代码，为服务器、数据库、用户和密码添加相应的值。

```ruby
require 'tiny_tds'
server = 'your_server.database.chinacloudapi.cn'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

puts "Reading data from table"
tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid"
result = client.execute(tsql)
result.each do |row|
    puts row
end
```

## <a name="run-the-code"></a>运行代码

1. 在命令提示符下运行以下命令：

   ```bash
   ruby sqltest.rb
   ```

2. 验证是否已返回前 20 行，然后关闭应用程序窗口。


## <a name="next-steps"></a>后续步骤
- [设计第一个 Azure SQL 数据库](sql-database-design-first-database.md)
- [TinyTDS 的 GitHub 存储库](https://github.com/rails-sqlserver/tiny_tds)
- [针对 TinyTDS 报告问题或提出问题](https://github.com/rails-sqlserver/tiny_tds/issues)
- [用于 SQL Server 的 Ruby 驱动程序](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)