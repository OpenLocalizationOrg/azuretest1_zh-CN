---
ms.openlocfilehash: 7d0884a05bc9f07a6200bba38f4ce549e5a0e8ae
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Ruby 使用 Mac OS x (Yosemite) TinyTDS 连接到 SQL 数据库" 
    description="使您可以在 Mac OS X (Yosemite) 来连接到 SQL Azure 数据库运行的 Ruby 代码示例。"
    services="sql-database" 
    documentationCenter="" 
    authors="ajlam" 
    manager="jeffreyg" 
    editor=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="07/20/2015" 
    ms.author="andrela"/>


# 在 Mac OS X (Yosemite) 使用 Ruby 连接到 SQL 数据库

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

本主题提供了在 Mac 计算机运行 Yosemite 连接到 SQL Azure 数据库数据库运行 Ruby 代码示例。

## 安装所需的模块

打开终端并安装以下︰

**1) Homebrew**︰ 从终端上运行下面的命令。 这将下载 Homebrew 程序包管理器在您的计算机上。 

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

**2) FreeTDS:**从终端上运行下面的命令。 这将在您的计算机上安装 FreeTDS，TinyTDS 工作的需要。

    brew install FreeTDS

**3) TinyTDS:**从终端上运行下面的命令。 这将在您的计算机上安装 TinyTDS。 

    sudo ARCHFLAGS="-arch x86_64" gem install tiny_tds

## 创建一个数据库，检索连接字符串

Ruby 的示例依赖于 AdventureWorks 示例数据库。 如果您已经没有 AdventureWorks，您可以看到如何创建它的以下主题︰[创建第一个 SQL Azure 数据库](sql-database-get-started.md)

该主题还说明了如何检索数据库连接字符串。

## 连接到 SQL 数据库

[TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds)函数用于连接到 SQL 数据库。

    require 'tiny_tds' 
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword', 
    host: 'yourserver.database.windows.net', port: 1433, 
    database: 'AdventureWorks', azure:true 

## 执行的 SELECT 语句，并检索结果集

[TinyTds::Result](https://github.com/rails-sqlserver/tiny_tds)函数用于检索结果集对 SQL 数据库的查询。 此函数接受一个查询并返回结果集。 结果集循环移动使用[result.each 做 | 行 |](https://github.com/rails-sqlserver/tiny_tds)。

    require 'tiny_tds'  
    print 'test'     
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword', 
    host: 'yourserver.database.windows.net', port: 1433, 
    database: 'AdventureWorks', azure:true 
    results = client.execute("select * from SalesLT.Product") 
    results.each do |row| 
    puts row 
    end 

## 插入行、 传递的参数，以及检索生成的主键值

该代码示例︰

- 将值插入行中的参数传递。
- 插入行。
- 检索生成的主键的值。

在 SQL 数据库中[标识](http://msdn.microsoft.com/library/ms186775.aspx)属性和[序列](http://msdn.microsoft.com/library/ff878058.aspx)对象可用来自动生成[主键值](http://msdn.microsoft.com/library/ms179610.aspx)。 

若要使用 Azure 的 TinyTDS，建议您执行多个`SET`语句来更改当前会话处理特定信息的方式。 建议使用`SET`中的代码示例提供了语句。 例如， `SET ANSI_NULL_DFLT_ON` ，即可创建允许空值，即使未显式说明列的为空性状态的新列。

要对齐与 Microsoft SQL Server[的日期时间](http://msdn.microsoft.com/library/ms187819.aspx)格式，请使用[strftime](http://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime)函数将强制转换为相应的日期时间格式。 

    require 'tiny_tds' 
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword', 
    host: 'yourserver.database.windows.net', port: 1433, 
    database: 'AdventureWorks', azure:true 
    results = client.execute("SET ANSI_NULLS ON")
    results = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
    results = client.execute("SET ANSI_NULL_DFLT_ON ON")
    results = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
    results = client.execute("SET ANSI_PADDING ON")
    results = client.execute("SET QUOTED_IDENTIFIER ON")
    results = client.execute("SET ANSI_WARNINGS ON")
    results = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")
    require 'date'
    t = Time.now
    curr_date = t.strftime("%Y-%m-%d %H:%M:%S.%L") 
    results = client.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) 
    OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, '#{curr_date}' )")
    results.each do |row| 
    puts row
    end
