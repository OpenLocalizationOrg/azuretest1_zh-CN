---
ms.openlocfilehash: cf1312572ddc8d08ab9bb5bf26952b16724e09ae
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="通过使用 Python pymssql 在 Ubuntu 上连接到 SQL 数据库" 
    description="提供了可用于连接到 SQL Azure 数据库 Python 代码示例。 在 Ubuntu Linux 客户端计算机上运行该示例。"
    services="sql-database" 
    documentationCenter="" 
    authors="meet-bhagdev" 
    manager="jeffreyg" 
    editor=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="04/27/2015" 
    ms.author="mebha"/>


# 在 Ubuntu Linux 上使用 Python 连接到 SQL 数据库


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


此主题提供 Python 代码示例运行 Ubuntu Linux 客户端计算机上，连接到 SQL Azure 数据库数据库。


## 要求


- [Python 2.7.6](https://www.python.org/download/releases/2.7.6/)。


### 安装所需的模块


打开终端并定位到计划在创建您的 python 脚本的目录。 输入以下命令以安装**FreeTDS**和**pymssql**。 pymssql 使用 FreeTDS 连接到 SQL 数据库。

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql


### 创建数据库并检索连接字符串


请参阅[入门指南](sql-database-get-started.md)以了解如何创建示例数据库并获取您的连接字符串。 请务必按照指南 》 以创建**AdventureWorks 数据库模板**。 下面显示的示例仅使用**AdventureWorks 架构**。 


## 连接到 SQL 数据库


[Pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html)函数用于连接到 SQL 数据库。

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


## 执行一个 SQL SELECT 语句

[Cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute)函数可以用于检索结果集对 SQL 数据库的查询。 此函数本质上是接受任何查询，并返回其可以循环访问通过[cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone)使用结果集。


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


## 插入行，传递参数，并检索生成的主键

在 SQL 数据库中[标识](https://msdn.microsoft.com/library/ms186775.aspx)属性和[SEQUENECE](https://msdn.microsoft.com/library/ff878058.aspx)的对象可用于自动生成[主键](https://msdn.microsoft.com/library/ms179610.aspx)的值。 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


## 事务


此代码示例演示如何使用中的交易记录的您︰


-开始事务

-插入数据的行

回滚事务要撤消插入


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()


 