---
ms.openlocfilehash: 32d34da179aae4bd442062a444bff93ff3d4e0da
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Windows 上使用 Node.js 连接到 SQL 数据库" 
    description="提供了可用于连接到 SQL Azure 数据库 Node.js 代码示例。 在 Windows 客户端计算机上运行该示例。"
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
    ms.date="07/30/2015" 
    ms.author="mebha"/>


# 在 Windows 上使用 Node.js 连接到 SQL 数据库


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


本主题提供了可用于连接到 SQL Azure 数据库 Node.js 代码示例。 Node.js 程序运行在 Windows 客户端计算机上。 若要管理连接，请使用 msnodesql 驱动程序。


## 要求


下面的软件项目必须存在于客户端开发计算机上。


-  Node.js –[版本 0.8.9 （32 位版本）](http://blog.nodejs.org/2012/09/11/node-v0-8-9-stable/)。 滚动，单击窗口安装程序下载的 32 位 x86，不为 Windows x64 安装 64 位。
- [Python 2.7.6](https://www.python.org/download/releases/2.7.6/)，x86 或 x64 安装程序。 
- [Visual C++ 2010年](https://app.vssps.visualstudio.com/profile/review?download=true&family=VisualStudioCExpress&release=VisualStudio2010&type=web&slcid=0x409&context=eyJwZSI6MSwicGMiOjEsImljIjoxLCJhbyI6MCwiYW0iOjEsIm9wIjpudWxsLCJhZCI6bnVsbCwiZmEiOjAsImF1IjpudWxsLCJjdiI6OTY4OTg2MzU1LCJmcyI6MCwic3UiOjAsImVyIjoxfQ2)-速成版是从 Microsoft 免费提供的。
- 在[SQL Server 2012年功能包](http://www.microsoft.com/download/details.aspx?id=29065)中找到 SQL Server 本机客户端 11.0-可用作为 Microsoft SQL Server 2012年的本机客户端。


### 安装所需的模块

一旦满足要求时，请确保位于 Node.js 版本 0.8.9。 这可以通过使用下面的命令从命令行终端检查︰ 节点-v。
<br>在**cmd.exe**命令行窗口中，导航到您项目目录-例如 C:\NodeJSSQLProject。 输入下面的命令显示的序列。 

    npm init
    npm install msnodesql

下一步定位到 node_modules\msnodesql 文件夹，然后运行 executible **msnodesql 0.2.1 v0.8 ia32** 。 按照安装向导中的步骤，完成后点击完成。 此时，您应该有 Node.js SQL Server 驱动程序安装。 请按照下面的步骤来获取您的连接字符串，然后您应该能够从 Node.js 应用程序连接到 Azure SQL 数据库。 

### 创建数据库并检索连接字符串
 
请参阅[入门主题](sql-database-get-started.md)以了解如何创建示例数据库和检索连接字符串。 请务必按照指南 》 以创建**AdventureWorks 数据库模板**。 仅适用于**AdventureWorks 架构**如下所示的示例。 


## 连接到 SQL 数据库


- 将下面的代码复制.js 文件位于项目目录中。


        var http = require('http');
        var sql = require('msnodesql');
        var http = require('http');
        var fs = require('fs');
        var useTrustedConnection = false;
        var conn_str = "Driver={SQL Server Native Client 11.0};Server=tcp:yourserver.database.windows.net;" + 
        (useTrustedConnection == true ? "Trusted_Connection={Yes};" : "UID=yourusername;PWD=yourpassword;") + 
        "Database={AdventureWorks};"
        sql.open(conn_str, function (err, conn) {
            if (err) {
                console.log("Error opening the connection!");
                return;
            }
            else
                console.log("Successfuly connected");
        }); 


- 现在，通过发出以下命令来运行.js 文件。


        node index.js


## 执行一个 SQL SELECT 语句


    var http = require('http');
    var sql = require('msnodesql');
    var http = require('http');
    var fs = require('fs');
    var useTrustedConnection = false;
    var conn_str = "Driver={SQL Server Native Client 11.0};Server=tcp:yourserver.database.windows.net;" + 
    (useTrustedConnection == true ? "Trusted_Connection={Yes};" : "UID=yourusername;PWD=yourpassword;") + 
    "Database={AdventureWorks};"
    sql.open(conn_str, function (err, conn) {
        if (err) {
            console.log("Error opening the connection!");
            return;
        }
        else
            console.log("Successfuly connected");
    
    
        conn.queryRaw("SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;", function (err, results) {
            if (err) {
                console.log("Error running query1!");
                return;
            }
            for (var i = 0; i < results.rows.length; i++) {
                console.log(results.rows[i]);
            }
        });
    });


## 插入行，传递参数，并检索生成的主键


    var http = require('http');
    var sql = require('msnodesql');
    var http = require('http');
    var fs = require('fs');
    var useTrustedConnection = false;
    var conn_str = "Driver={SQL Server Native Client 11.0};Server=tcp:yourserver.database.windows.net;" + 
    (useTrustedConnection == true ? "Trusted_Connection={Yes};" : "UID=yourusername;PWD=yourpassword;") + 
    "Database={AdventureWorks};"
    sql.open(conn_str, function (err, conn) {
        if (err) {
            console.log("Error opening the connection!");
            return;
        }
        else
            console.log("Successfuly connected");
    
    
        conn.queryRaw("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)", function (err, results) {
            if (err) {
                console.log("Error running query!");
                return;
            }
            for (var i = 0; i < results.rows.length; i++) {
                console.log("Product ID Inserted : "+results.rows[i]);
            }
        });
    });


## 事务


方法**conn.beginTransactions**在 SQL Azure 数据库中将无效。 相反，请按照以下代码示例执行 SQL 数据库中的事务。


    var http = require('http');
    var sql = require('msnodesql');
    var http = require('http');
    var fs = require('fs');
    var useTrustedConnection = false;
    var conn_str = "Driver={SQL Server Native Client 11.0};Server=tcp:yourserver.database.windows.net;" + 
    (useTrustedConnection == true ? "Trusted_Connection={Yes};" : "UID=yourusername;PWD=yourpassword;") + 
    "Database={AdventureWorks};"
    sql.open(conn_str, function (err, conn) {
        if (err) {
            console.log("Error opening the connection!");
            return;
        }
        else
            console.log("Successfuly connected");
    
    
        conn.queryRaw("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New ', 'SQLEXPRESS New', 1, 1, CURRENT_TIMESTAMP)", function (err, results) {
            if (err) {
                console.log("Error running query!");
                return;
            }
            for (var i = 0; i < results.rows.length; i++) {
                console.log("Product ID Inserted : "+results.rows[i]);
            }
        });
        
        conn.queryRaw("ROLLBACK TRANSACTION; ", function (err, results) {
                if (err) {
                console.log("Rollback failed");
                return;
            }
            });
    });


## 存储的过程


才能运行此代码示例，您必须首先具有或创建输入不带参数的存储的过程。 与 SQL Server 管理 Studio (SSMS.exe) 之类的工具，可以创建一个存储的过程。


    var http = require('http');
    var sql = require('msnodesql');
    var http = require('http');
    var fs = require('fs');
    var useTrustedConnection = false;
    var conn_str = "Driver={SQL Server Native Client 11.0};Server=tcp:yourserver.database.windows.net;" + 
    (useTrustedConnection == true ? "Trusted_Connection={Yes};" : "UID=yourusername;PWD=yourpassword;") + 
    "Database={AdventureWorks};"
    sql.open(conn_str, function (err, conn) {
        if (err) {
            console.log("Error opening the connection!");
            return;
        }
        else
            console.log("Successfuly connected");
        
        conn.query("exec NameOfStoredProcedure", function (err, results) {
            if (err) {
            console.log("Error running query8!");
            return;
        }
        });
    });

 
