---
ms.openlocfilehash: 37343a049e698a7b64b9e710ac704430f4bc568e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Ruby 使用在 Windows 上的 TinyTDS 连接到 SQL 数据库" 
    description="使您可以连接到 SQL Azure 数据库在 Windows 运行的 Ruby 代码示例。"
    services="sql-database" 
    documentationCenter="" 
    authors="meet-bhagdev" 
    manager="jeffreyg" 
    editor=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="mebha"/>


# 在 Windows 上使用 Ruby 连接到 SQL 数据库

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

本主题提供在运行 Windows 8.1 到 SQL Azure 数据库的数据库连接的 Windows 计算机运行的 Ruby 代码示例。

## 安装所需的模块

打开终端并安装以下︰

**1) 拼音︰**如果您的计算机没有拼音，请安装它。 对于新的 ruby 用户，我们建议使用 Ruby 2.1.X 的安装程序。 这提供了稳定的语言和大量的软件包 (gem) 都兼容，并更新列表。 [转 Ruby 的下载页面](http://rubyinstaller.org/downloads/)，下载相应的 2.1.x 安装程序。 示例如果要在 64 位计算机上，下载**Ruby 2.1.6 (x64)**安装程序。
<br/><br/>一旦下载安装程序，请执行以下操作︰ 


- 双击该文件以启动安装程序。

- 选择您的语言，并同意这些条款。

- 在安装设置屏幕中，选择*添加拼音到您的路径的可执行文件*和*关联具有此 Ruby 安装.rb 和.rbw 文件*旁边的复选框。


**2) DevKit:** [RubyInstaller 页面](http://rubyinstaller.org/downloads/)中下载 DevKit

下载完成后，请执行以下操作︰


- 双击该文件。 将要求您提取的文件的位置。

- 单击"..."按钮，然后选择"C:\DevKit"。 您可能需要首先通过单击"新建文件夹"来创建此文件夹。

- 单击"确定"，然后"提取"，以解压文件。


现在打开命令提示符并输入以下命令︰

    > chdir C:\DevKit
    > ruby dk.rb init
    > ruby dk.rb install

现在有一个完备的 Ruby 和 RubyGems ！


**3) TinyTDS:**到 C:\DevKit，并运行下面的命令从终端的导航。 这将在您的计算机上安装 TinyTDS。 

    gem inst tiny_tds --pre

## 创建一个数据库，检索连接字符串

Ruby 的示例依赖于`AdventureWorks`示例数据库。 如果您还没有`AdventureWorks`，您可以看到如何通过访问[创建第一个 SQL Azure 数据库](sql-database-get-started.md)创建它。

该主题还说明了如何检索数据库连接字符串。

## 连接到 SQL 数据库

[TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds)函数用于连接到 SQL 数据库。

    require 'tiny_tds' 
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword', 
    host: 'yourserver.database.windows.net', port: 1433, 
    database: 'AdventureWorks', azure:true 

## 执行的 SELECT 语句，并检索结果集

复制并将下面的代码粘贴到一个空文件。 Test.rb 调用它。 然后执行从命令提示符输入以下命令︰

    ruby test.rb

在代码示例中， [TinyTds::Result](https://github.com/rails-sqlserver/tiny_tds)函数用于检索结果集对 SQL 数据库的查询。 此函数接受一个查询并返回结果集。 结果集循环移动使用[result.each 做 | 行 |](https://github.com/rails-sqlserver/tiny_tds)。

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
