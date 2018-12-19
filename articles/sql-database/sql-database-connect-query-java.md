---
title: 使用 Java 查询 Azure SQL 数据库 | Microsoft Docs
description: 本主题介绍如何使用 Java 创建可连接到 Azure SQL 数据库的程序并使用 Transact-SQL 语句对其进行查询。
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: java
ms.topic: quickstart
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: digimobile
origin.date: 11/01/2018
ms.date: 12/03/2018
ms.openlocfilehash: 655e94fe9eb4423fd6c7a9b78e3434d09fb02195
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52672540"
---
# <a name="quickstart-use-java-to-query-an-azure-sql-database"></a>快速入门：使用 Java 查询 Azure SQL 数据库

本快速入门演示了如何使用 [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) 连接到 Azure SQL 数据库，然后使用 Transact-SQL 语句查询数据。

## <a name="prerequisites"></a>先决条件

若要完成本快速入门，请确保符合以下先决条件：

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- 针对用于本快速入门的计算机的公共 IP 地址制定[服务器级防火墙规则](sql-database-get-started-portal-firewall.md)。

- 已为操作系统安装 Java 和相关软件：

    - **MacOS**：安装 Homebrew 和 Java，然后再安装 Maven。 请参阅[步骤 1.2 和 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/)。
    - **Ubuntu**：安装 Java 开发工具包，并安装 Maven。 请参阅[步骤 1.2、1.3 和 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/)。
    - **Windows**：安装 Java 开发工具包和 Maven。 请参阅[步骤 1.2 和 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/)。    

## <a name="sql-server-connection-information"></a>SQL Server 连接信息

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-maven-project-and-dependencies"></a>**创建 Maven 项目和依赖项**
1. 从终端创建一个名为 **sqltest** 的新 Maven 项目。 

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

2. 出现提示时输入“Y”。
3. 将目录更改为 **sqltest**，然后使用常用文本编辑器打开 pom.xml。  使用以下代码，将 **Microsoft JDBC Driver for SQL Server** 添加到项目的依赖项：

   ```xml
   <dependency>
       <groupId>com.microsoft.sqlserver</groupId>
       <artifactId>mssql-jdbc</artifactId>
       <version>6.4.0.jre8</version>
   </dependency>
   ```

4. 另外，在 pom.xml 中添加项目的以下属性。  如果没有 properties 节，可以将其添加到 dependencies 后面。

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

5. 保存并关闭 pom.xml。

## <a name="insert-code-to-query-sql-database"></a>插入用于查询 SQL 数据库的代码

1. 应该已在 Maven 项目中有了一个名为 ***App.java*** 的文件，其位置为：\sqltest\src\main\java\com\sqlsamples\App.java

2. 打开该文件，将其内容替换为以下代码，并为服务器、数据库、用户和密码添加相应的值。

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.DriverManager;

   public class App {

    public static void main(String[] args) {
    
        // Connect to database
           String hostName = "your_server.database.chinacloudapi.cn";
           String dbName = "your_database";
           String user = "your_username";
           String password = "your_password";
           String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.chinacloudapi.cn;loginTimeout=30;", hostName, dbName, user, password);
           Connection connection = null;

           try {
                   connection = DriverManager.getConnection(url);
                   String schema = connection.getSchema();
                   System.out.println("Successful connection - Schema: " + schema);

                   System.out.println("Query data example:");
                   System.out.println("=========================================");

                   // Create and execute a SELECT SQL statement.
                   String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName " 
                       + "FROM [SalesLT].[ProductCategory] pc "  
                       + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";
                
                   try (Statement statement = connection.createStatement();
                       ResultSet resultSet = statement.executeQuery(selectSql)) {

                           // Print results from select statement
                           System.out.println("Top 20 categories:");
                           while (resultSet.next())
                           {
                               System.out.println(resultSet.getString(1) + " "
                                   + resultSet.getString(2));
                           }
                    connection.close();
                   }                   
           }
           catch (Exception e) {
                   e.printStackTrace();
           }
       }
   }
   ```

## <a name="run-the-code"></a>运行代码

1. 在命令提示符下运行以下命令：

   ```bash
   mvn package
   mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

2. 验证是否已返回前 20 行，然后关闭应用程序窗口。


## <a name="next-steps"></a>后续步骤
- [设计第一个 Azure SQL 数据库](sql-database-design-first-database.md)
- [用于 SQL Server 的 Microsoft JDBC 驱动程序](https://github.com/microsoft/mssql-jdbc)
- [报告问题/提出问题](https://github.com/microsoft/mssql-jdbc/issues)
