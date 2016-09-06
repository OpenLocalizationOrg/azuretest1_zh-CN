---
ms.openlocfilehash: 07d93fad400800726622d0b014f1b378493e3423
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="PHP 在 Windows 向 SQL 数据库 |Microsoft Azure"
    description="提供一个示例 PHP 程序具有瞬时性故障处理，从 Windows 客户端连接到 SQL Azure 数据库并提供必需的软件组件所需的客户端的链接。"
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
    ms.date="07/21/2015"
    ms.author="mebha"/>


# 在瞬时性故障处理 Windows 上使用 PHP 连接到 SQL 数据库


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


本主题说明了如何可以对 SQL Azure 数据库连接从客户端应用程序在 Windows 运行的 PHP 编写。


[AZURE.INCLUDE [sql-database-develop-includes-prerequisites-php-windows](../../includes/sql-database-develop-includes-prerequisites-php-windows.md)]


## 创建数据库并检索连接字符串


请参阅[获取启动主题](sql-database-get-started.md)以了解如何创建示例数据库和检索连接字符串。 请务必按照指南 》 以创建**AdventureWorks 数据库模板**。 下面显示的示例仅使用**AdventureWorks 架构**。 


## 连接并查询数据库 

演示程序被设计，以便在连接尝试期间暂时的错误会导致重试。 但查询命令期间暂时的错误会导致程序放弃连接并创建一个新的连接之前重试查询命令。 我们既不建议也不 disrecommend 这种设计选择。 演示程序演示了一些可供您的设计灵活性。

<br>此代码示例的长度是主要由于捕获异常逻辑。 该 Program.cs 文件有较短版本此[这里](sql-database-develop-php-simple-windows.md)。
<br>Main 方法是在 Program.cs 中。 调用堆栈运行，如下所示︰
* 主调用 ConnectAndQuery。
* ConnectAndQuery 调用 EstablishConnection。
* EstablishConnection 调用 IssueQueryCommand。

[Sqlsrv_query()](http://php.net/manual/en/function.sqlsrv-query.php)函数可以用于检索结果集对 SQL 数据库的查询。 此函数本质上是接受任何查询和连接对象并返回其可以循环访问通过[sqlsrv_fetch_array()](http://php.net/manual/en/function.sqlsrv-fetch-array.php)使用结果集。

    <?php
        // Variables to tune the retry logic.  
        $connectionTimeoutSeconds = 30;  // Default of 15 seconds is too short over the Internet, sometimes.
        $maxCountTriesConnectAndQuery = 3;  // You can adjust the various retry count values.
        $secondsBetweenRetries = 4;  // Simple retry strategy.
        $errNo = 0;
        $serverName = "tcp:yourdatabase.database.windows.net,1433";
        $connectionOptions = array("Database"=>"AdventureWorks",
           "Uid"=>"yourusername", "PWD"=>"yourpassword", "LoginTimeout" => $connectionTimeoutSeconds);
        $conn;
        $errorArr = array();
        for ($cc = 1; $cc <= $maxCountTriesConnectAndQuery; $cc++)
        {
            $errorArr = array();
            $ctr = 0;
            // [A.2] Connect, which proceeds to issue a query command. 
            $conn = sqlsrv_connect($serverName, $connectionOptions);  
            if( $conn == true)
            {
                echo "Connection was established"; 
                echo "<br>";
         
                $tsql = "SELECT [CompanyName] FROM SalesLT.Customer";
                $getProducts = sqlsrv_query($conn, $tsql);
                if ($getProducts == FALSE)
                    die(FormatErrors(sqlsrv_errors()));
                $productCount = 0;
                $ctr = 0;
                while($row = sqlsrv_fetch_array($getProducts, SQLSRV_FETCH_ASSOC))
                {   
                    $ctr++;
                    echo($row['CompanyName']);
                    echo("<br/>");
                    $productCount++;
                    if($ctr>10)
                        break;
                }
                sqlsrv_free_stmt($getProducts);
                break;
            }
            // Adds any the error codes from the SQL Exception to an array.
            else {  
                if( ($errors = sqlsrv_errors() ) != null) {
                    foreach( $errors as $error ) {
                        $errorArr[$ctr] = $error['code'];
                        $ctr = $ctr + 1;
                    }
                }
                $isTransientError = TRUE;
                // [A.4] Check whether sqlExc.Number is on the whitelist of transients.
                $isTransientError = IsTransientStatic($errorArr);
                if ($isTransientError == TRUE)  // Is a static persistent error...
                { 
                    echo("Persistent error suffered, SqlException.Number==". $errorArr[0].". Program Will terminate."); 
                    echo "<br>";
                    // [A.5] Either the connection attempt or the query command attempt suffered a persistent SqlException.
                    // Break the loop, let the hopeless program end.
                    exit(0);
                }
                // [A.6] The SqlException identified a transient error from an attempt to issue a query command.
                // So let this method reloop and try again. However, we recommend that the new query
                // attempt should start at the beginning and establish a new connection.
                if ($cc >= $maxCountTriesConnectAndQuery)
                {
                    echo "Transient errors suffered in too many retries - " . $cc ." Program will terminate.";
                    echo "<br>";
                    exit(0);
                }
                echo("Transient error encountered.  SqlException.Number==". $errorArr[0]. " . Program might retry by itself.");  
                echo "<br>";
                echo $cc . " Attempts so far. Might retry.";
                echo "<br>";
                // A very simple retry strategy, a brief pause before looping. This could be changed to exponential if you want.
                sleep(1*$secondsBetweenRetries);
            }
            // [A.3] All has gone well, so let the program end.
        }
        function IsTransientStatic($errorArr) {
            $arrayOfTransientErrorNumbers = array(4060, 10928, 10929, 40197, 40501, 40613);
            $result = array_intersect($arrayOfTransientErrorNumber, $errorArr);
            $count = count($result);
            if($count >= 0) //change to > 0 later.
                return TRUE;
        }
    ?>
    
## 其他阅读材料


关于 PHP 安装和使用的详细信息，请参阅[使用 PHP 中访问 SQL Server 数据库](http://technet.microsoft.com/library/cc793139.aspx)。

 
