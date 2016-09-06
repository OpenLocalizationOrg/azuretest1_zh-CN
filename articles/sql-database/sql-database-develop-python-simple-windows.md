---
ms.openlocfilehash: 34cc6d0382967cc04c0983e3ff1a8859cf4786df
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Windows 上使用 Python 连接到 SQL 数据库" 
    description="介绍可用于从 Windows 客户端连接到 SQL Azure 数据库的 Python 代码示例。 此示例使用 pymssql 驱动程序。"
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
    ms.date="07/20/2015" 
    ms.author="mebha"/>


# 在 Windows 上使用 Python 连接到 SQL 数据库


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


本主题提供一个用 Python 编写的代码示例。 在 Windows 计算机上运行该示例。 此示例使用**pymssql**驱动程序连接到 SQL Azure 数据库。


## 要求


- [2.7.6 Python](https://www.python.org/download/releases/2.7.6/)


### 安装所需的模块


安装[pymssql](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql)。 

请确保选择正确 whl 文件。

例如︰ 如果您在 64 位计算机上使用 Python 2.7 选择︰ pymssql‑2.1.1‑cp27‑none‑win_amd64.whl。
一旦您下载的.whl 文件放置在 c: / Python27 文件夹。

现在安装了使用来自命令行的 pip 的 pymssql 驱动程序。 到 c: / Python27，并运行以下的 cd
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

若要启用使用 pip 的指导可以找到[这里](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)


## 创建数据库并检索连接字符串


请参阅[获取启动主题](sql-database-get-started.md)以了解如何创建示例数据库和检索连接字符串。 请务必按照指南 》 以创建**AdventureWorks 数据库模板**。 下面显示的示例仅使用**AdventureWorks 架构**。 


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

在 SQL 数据库中[标识](https://msdn.microsoft.com/library/ms186775.aspx)属性和[序列](https://msdn.microsoft.com/library/ff878058.aspx)对象可用来自动生成[主键](https://msdn.microsoft.com/library/ms179610.aspx)的值。 


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

 
