---
ms.openlocfilehash: 30e224d78f8d108322cda7a0240852f20942e7e0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Mac OS X 上的单调的 Node.js 连接到 SQL 数据库" 
    description="提供了可用于连接到 SQL Azure 数据库 Node.js 代码示例。 此示例使用繁琐的驱动程序来连接。"
    services="sql-database" 
    documentationCenter="" 
    authors="meet-bhagdev" 
    manager="jeffreyg" 
    editor=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="07/20/2015" 
    ms.author="mebha"/>


# 使用 Mac OS X 上的单调的 Node.js 连接到 SQL 数据库


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


本主题提供了在 Mac OS X 运行 Node.js 代码示例。该示例使用繁琐的驱动程序连接到 SQL Azure 数据库。


## 所需的软件项目


安装**节点**，除非您计算机上已安装。


在 OSX 10.10 Yosemite 上安装 node.js 您可以下载一个预编译的二进制包这样出色且易于安装。 [请访问 nodejs.org](http://nodejs.org/) ，然后单击安装按钮下载最新的包。

安装包从.dmg 随笔安装向导会安装**节点**和**npm**，npm 是它有助于节点程序包管理器的 node.js 其他程序包的安装。


您的计算机配置**节点**和**npm**后，导航到计划要创建 Node.js 项目，一个目录并输入以下命令。


    npm init
    npm install tedious


**npm init**创建节点的项目。 若要在项目创建期间保留默认设置，请按 enter 键之前创建的项目。 现在您看到您的项目目录中的**package.json**文件。


### 创建 AdventureWorks 数据库


本主题中的代码示例要求**AdventureWorks**测试数据库。 如果您没有一个，请参阅[开始使用 SQL 数据库](sql-database-get-started.md)。 请务必按照指南 》 以创建**AdventureWorks 数据库模板**。 下面显示的示例只使用**AdventureWorks 架构**。 


## 连接到 SQL 数据库

[新的连接](http://pekim.github.io/tedious/api-connection.html)函数用于连接到 SQL 数据库。

    var Connection = require('tedious').Connection;
    var config = {
        userName: 'yourusername',
        password: 'yourpassword',
        server: 'yourserver.database.windows.net',
        // If you are on Microsoft Azure, you need this:
        options: {encrypt: true, database: 'AdventureWorks'}
    };
    var connection = new Connection(config);
    connection.on('connect', function(err) {
    // If no error, then good to proceed.
        console.log("Connected");
    });


## 执行 SQL 选择


使用[新的 Request()](http://pekim.github.io/tedious/api-request.html)函数执行所有 SQL 语句。 如果该语句返回行，如一条 select 语句，您可以检索它们使用[request.on()](http://pekim.github.io/tedious/api-request.html)函数。 如果不有任何行， [request.on()](http://pekim.github.io/tedious/api-request.html)函数返回空列表。


    var Connection = require('tedious').Connection;
    var Request = require('tedious').Request;
    var TYPES = require('tedious').TYPES;
    var config = {
        userName: 'yourusername',
        password: 'yourpassword',
        server: 'yourserver.database.windows.net',
        // When you connect to Azure SQL Database, you need these next options.
        options: {encrypt: true, database: 'AdventureWorks'}
    };
    var connection = new Connection(config);
    connection.on('connect', function(err) {
        // If no error, then good to proceed.
        console.log("Connected");
        executeStatement();
    });
    
    
    function executeStatement() {
        request = new Request("SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;", function(err) {
        if (err) {
            console.log(err);} 
        });
        var result = "";
        request.on('row', function(columns) {
            columns.forEach(function(column) {
              if (column.value === null) {
                console.log('NULL');
              } else {
                result+= column.value + " ";
              }
            });
            console.log(result);
            result ="";
        });
    
        request.on('done', function(rowCount, more) {
        console.log(rowCount + ' rows returned');
        });
        connection.execSql(request);
    }


## 插入行，应用参数，并检索生成的主键


在 SQL 数据库中[标识](https://msdn.microsoft.com/library/ms186775.aspx)属性和[序列](https://msdn.microsoft.com/library/ff878058.aspx)对象可用来自动生成[主键](https://msdn.microsoft.com/library/ms179610.aspx)的值。 在本示例中将了解如何执行的 insert 语句，则安全地传递参数的防止 SQL 注入和检索自动生成的主键值。


本节中的代码示例适用于一个 SQL INSERT 语句的参数。 生成的主键值检索程序。


    var Connection = require('tedious').Connection;
    var Request = require('tedious').Request
    var TYPES = require('tedious').TYPES;
    var config = {
        userName: 'yourusername',
        password: 'yourpassword',
        server: 'yourserver.database.windows.net',
        // If you are on Azure SQL Database, you need these next options.
        options: {encrypt: true, database: 'AdventureWorks'}
    };
    var connection = new Connection(config);
    connection.on('connect', function(err) {
        // If no error, then good to proceed.
        console.log("Connected");
        executeStatement1();
    });
    
    
    function executeStatement1() {
        request = new Request("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES (@Name, @Number, @Cost, @Price, CURRENT_TIMESTAMP);", function(err) {
         if (err) {
            console.log(err);} 
        });
        request.addParameter('Name', TYPES.NVarChar,'SQL Server Express 2014');
        request.addParameter('Number', TYPES.NVarChar , 'SQLEXPRESS2014');
        request.addParameter('Cost', TYPES.Int, 11);
        request.addParameter('Price', TYPES.Int,11);
        request.on('row', function(columns) {
            columns.forEach(function(column) {
              if (column.value === null) {
                console.log('NULL');
              } else {
                console.log("Product id of inserted item is " + column.value);
              }
            });
        });     
        connection.execSql(request);
    }
