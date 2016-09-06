---
ms.openlocfilehash: 8f75f28a20dce21162982092db70c21ccf950984
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="PHP 在 Windows 向 SQL 数据库 |Microsoft Azure"
    description="提供一个示例 PHP 程序从 Windows 客户端，连接到 SQL Azure 数据库并提供必需的软件组件所需的客户端的链接。"
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jeffreyg"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="07/20/2015"
    ms.author="mebha"/>


# 通过在 Windows 中使用 PHP 来连接到 SQL 数据库


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


本主题说明了如何可以对 SQL Azure 数据库连接从客户端应用程序在 Windows 运行的 PHP 编写。


[AZURE.INCLUDE [sql-database-develop-includes-prerequisites-php-windows](../../includes/sql-database-develop-includes-prerequisites-php-windows.md)]


## 创建数据库并检索连接字符串


请参阅[获取启动主题](sql-database-get-started.md)以了解如何创建示例数据库和检索连接字符串。 请务必按照指南 》 以创建**AdventureWorks 数据库模板**。 下面显示的示例仅使用**AdventureWorks 架构**。 


## 连接到 SQL 数据库


此**OpenConnection**函数调用的所有函数，请按照中的顶部附近。


    function OpenConnection()
    {
        try
        {
            $serverName = "tcp:myserver.database.windows.net,1433";
            $connectionOptions = array("Database"=>"AdventureWorks",
                "Uid"=>"MyUser", "PWD"=>"MyPassword");
            $conn = sqlsrv_connect($serverName, $connectionOptions);
            if($conn == false)
                die(FormatErrors(sqlsrv_errors()));
        }
        catch(Exception $e)
        {
            echo("Error!");
        }
    }


## 执行查询并检索结果集

[Sqlsrv_query()](http://php.net/manual/en/function.sqlsrv-query.php)函数可以用于检索结果集对 SQL 数据库的查询。 此函数本质上是接受任何查询和连接对象并返回其可以循环访问通过[sqlsrv_fetch_array()](http://php.net/manual/en/function.sqlsrv-fetch-array.php)使用结果集。

    function ReadData()
    {
        try
        {
            $conn = OpenConnection();
            $tsql = "SELECT [CompanyName] FROM SalesLT.Customer";
            $getProducts = sqlsrv_query($conn, $tsql);
            if ($getProducts == FALSE)
                die(FormatErrors(sqlsrv_errors()));
            $productCount = 0;
            while($row = sqlsrv_fetch_array($getProducts, SQLSRV_FETCH_ASSOC))
            {
                echo($row['CompanyName']);
                echo("<br/>");
                $productCount++;
            }
            sqlsrv_free_stmt($getProducts);
            sqlsrv_close($conn);
        }
        catch(Exception $e)
        {
            echo("Error!");
        }
    }
    

## 插入行，传递参数，并检索生成的主键


在 SQL 数据库中[标识](https://msdn.microsoft.com/library/ms186775.aspx)属性和[序列](https://msdn.microsoft.com/library/ff878058.aspx)对象可用来自动生成[主键](https://msdn.microsoft.com/library/ms179610.aspx)的值。 


    function InsertData()
    {
        try
        {
            $conn = OpenConnection();

            $tsql = "INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT            INSERTED.ProductID VALUES ('SQL Server 1', 'SQL Server 2', 0, 0, getdate())";
            //Insert query
            $insertReview = sqlsrv_query($conn, $tsql);
            if($insertReview == FALSE)
                die(FormatErrors( sqlsrv_errors()));
            echo "Product Key inserted is :";   
            while($row = sqlsrv_fetch_array($insertReview, SQLSRV_FETCH_ASSOC))
            {   
                echo($row['ProductID']);
            }
            sqlsrv_free_stmt($insertReview);
            sqlsrv_close($conn);
        }
        catch(Exception $e)
        {
            echo("Error!");
        }
    }

## 事务


此代码示例演示如何使用中的交易记录的您︰

-开始事务

-插入的数据行、 更新数据的另一个行

-提交您的交易如果成功的插入和更新记录和回滚事务如果其中之一不是


    function Transactions()
    {
        try
        {
            $conn = OpenConnection();

            if (sqlsrv_begin_transaction($conn) == FALSE)
                die(FormatErrors(sqlsrv_errors()));

            $tsql1 = "INSERT INTO SalesLT.SalesOrderDetail (SalesOrderID,OrderQty,ProductID,UnitPrice) 
            VALUES (71774, 22, 709, 33)";
            $stmt1 = sqlsrv_query($conn, $tsql1);
            
            /* Set up and execute the second query. */
            $tsql2 = "UPDATE SalesLT.SalesOrderDetail SET OrderQty = (OrderQty + 1) WHERE ProductID = 709";
            $stmt2 = sqlsrv_query( $conn, $tsql2);
            
            /* If both queries were successful, commit the transaction. */
            /* Otherwise, rollback the transaction. */
            if($stmt1 && $stmt2)
            {
                   sqlsrv_commit($conn);
                   echo("Transaction was commited");
            }
            else
            {
                sqlsrv_rollback($conn);
                echo "Transaction was rolled back.\n";
            }
            /* Free statement and connection resources. */
            sqlsrv_free_stmt( $stmt1);
            sqlsrv_free_stmt( $stmt2);
        }
        catch(Exception $e)
        {
            echo("Error!");
        }
    }


## 其他阅读材料


关于 PHP 安装和使用的详细信息，请参阅[使用 PHP 中访问 SQL Server 数据库](http://technet.microsoft.com/library/cc793139.aspx)。

 
