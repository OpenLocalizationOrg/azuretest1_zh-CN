---
ms.openlocfilehash: 5e025d3f14c4b89c3b79286f5fbd85e5e04459e6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="通过 Java 中使用 Windows 上的 JDBC 连接到 SQL 数据库" 
    description="介绍了可用于连接到 SQL Azure 数据库的 Java 代码示例。 该示例使用 JDBC，并且它在 Windows 客户端计算机上运行。"
    services="sql-database" 
    documentationCenter="" 
    authors="LuisBosquez" 
    manager="jeffreyg" 
    editor="genemi"/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="06/25/2015" 
    ms.author="lbosq"/>


# 通过 Java 中使用 Windows 上的 JDBC 连接到 SQL 数据库


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


本主题提供了可用于连接到 SQL Azure 数据库的 Java 代码示例。 Java 示例依赖于在 Java 开发工具箱 (JDK) 版本 1.8。 该示例使用的 JDBC 驱动程序连接到 SQL Azure 数据库。


## 要求


- [SQL Server 的 SQL JDBC 4 Microsoft JDBC 驱动程序](http://www.microsoft.com/download/details.aspx?displaylang=en&id=11774)。
- 运行[Java 开发工具包 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)任何操作系统平台。
- 在 SQL Azure 上现有数据库。 请参阅[入门主题](sql-database-get-started.md)以了解如何创建示例数据库和检索连接字符串。


## 测试环境


本主题中的 Java 代码示例假定您的 SQL Azure 数据库数据库中已存在以下测试表。


<!--
Could this instead be a #tempPerson table, so that the Java code sample could be fully self-sufficient and be runnable (with automatic cleanup)?
-->


    CREATE TABLE Person
    (
        id         INT    PRIMARY KEY    IDENTITY(1,1),
        firstName  VARCHAR(32),
        lastName   VARCHAR(32),
        age        INT
    );


## SQL 数据库的连接字符串


此代码示例创建`Connection`对象使用的连接字符串。 您可以通过使用[Azure 预览门户](http://portal.azure.com/)找到连接字符串。 有关查找连接字符串的详细信息，请参阅[创建第一个 SQL Azure 数据库](sql-database-get-started.md)。


## Java 代码示例


该节所包含的大量 Java 代码示例。 它具有注释，指示您想复制粘贴小 Java 中的段所提供的后续部分。 本节中的示例可以编译并运行即使没有复制-和-粘贴附近注释，但它将仅连接，然后结束。 注释将发现如下所示︰


1. `// INSERT two rows into the table.`
2. `// TRANSACTION and commit for an UPDATE.`
3. `// SELECT rows from the table.`


下一步还有大量的 Java 代码示例。 此示例包括`main`函数的`SQLDatabaseTest`类。


    import java.sql.*;
    import com.microsoft.sqlserver.jdbc.*;
    
    public class SQLDatabaseTest {
    
        public static void main(String[] args) {
            String connectionString =
                "jdbc:sqlserver://your_server.database.windows.net:1433;" 
                + "database=your_database;"
                + "user=your_user@your_server;"
                + "password={your_password};"
                + "encrypt=true;"
                + "trustServerCertificate=false;"
                + "hostNameInCertificate=*.database.windows.net;"
                + "loginTimeout=30;"; 
    
            // Declare the JDBC objects.
            Connection connection = null;
            Statement statement = null;
            ResultSet resultSet = null;
            PreparedStatement prepsInsertPerson = null;
            PreparedStatement prepsUpdateAge = null;
    
            try {
                connection = DriverManager.getConnection(connectionString);
    
                // INSERT two rows into the table.
                // ...
    
                // TRANSACTION and commit for an UPDATE.
                // ...
    
                // SELECT rows from the table.
                // ...
            }
            catch (Exception e) {
                e.printStackTrace();
            }
            finally {
                // Close the connections after the data has been handled.
                if (prepsInsertPerson != null) try { prepsInsertPerson.close(); } catch(Exception e) {}
                if (prepsUpdateAge != null) try { prepsUpdateAge.close(); } catch(Exception e) {}
                if (resultSet != null) try { resultSet.close(); } catch(Exception e) {}
                if (statement != null) try { statement.close(); } catch(Exception e) {}
                if (connection != null) try { connection.close(); } catch(Exception e) {}
            }
        }
    }


当然，实际运行上述 Java 代码示例，您必须将您的真实值放入连接字符串来替换占位符︰


- 您的服务器
- your_database
- your_user
- your_password


## 向表中插入两行


此 Java 段发出人表中插入两行的插入事务处理 SQL 语句。 一般规则是，如下所示︰


1. 生成`PreparedStatement`对象通过使用`Connection`对象。
 - 我们将参数包括`Statement.RETURN_GENERATED_KEYS`，这样以后我们可获得自动生成的**id**的键值对的值。
2. 调用`execute`方法在`PreparedStatement`对象。
3. 获取的数值由自动生成主键，使用`PreparedStatement`对象。
 - 这与在用户表中的**id**列的 AUTO_INCREMENT 规范


复制并粘贴此短 Java 划分在此可以看到注释的主代码示例`// INSERT two rows into the table.`。


    // Create and execute an INSERT SQL prepared statement.
    String insertSql = "INSERT INTO Person (firstName, lastName, age) VALUES "
        + "('Bill', 'Gates', 59), "
        + "('Steve', 'Ballmer', 59);";
    
    prepsInsertPerson = connection.prepareStatement(
        insertSql,
        Statement.RETURN_GENERATED_KEYS);
    prepsInsertPerson.execute();
    // Retrieve the generated key from the insert.
    resultSet = prepsInsertPerson.getGeneratedKeys();
    // Iterate through the set of generated keys.
    while (resultSet.next()) {
        System.out.println("Generated: " + resultSet.getString(1));
    }


## 交易记录和提交以进行更新


下面的 Java 代码段发出的事务处理 SQL UPDATE 语句增加`age`的人表中每一行的值。 一般规则是，如下所示︰


1. `setAutoCommit`调用方法以防止更新在数据库中自动提交。
2. `executeUpdate`调用方法以在事务的上下文中执行更新。
3. `commit`调用方法来显式提交事务。


复制并粘贴此短 Java 划分在此可以看到注释的主代码示例`// TRANSACTION and commit for an UPDATE.`。


    // Set AutoCommit value to false to execute a single transaction at a time.
    connection.setAutoCommit(false);
    
    // Write the SQL Update instruction and get the PreparedStatement object.
    String transactionSql = "UPDATE Person SET Person.age = Person.age + 1;";
    prepsUpdateAge = connection.prepareStatement(transactionSql);
    
    // Execute the statement.
    prepsUpdateAge.executeUpdate();
    
    //Commit the transaction.
    connection.commit();
    
    // Return the AutoCommit value to true.
    connection.setAutoCommit(true);


## 从表中选择行


此 Java 段执行事务处理 SQL SELECT 语句查看人员表中所有已更新的行。 一般规则是，如下所示︰


1. 生成`Statement`对象通过使用`Connection`对象。
2. 生成`ResultSet`对象通过使用`Statement`对象。
3. 对周围循环`resultSet.next`循环返回的所有行。


复制并粘贴此短 Java 划分在此可以看到注释的主代码示例`// SELECT rows from a table.`。


    // Create and execute a SELECT SQL statement.
    String selectSql = "SELECT firstName, lastName, age FROM dbo.Person";
    statement = connection.createStatement();
    resultSet = statement.executeQuery(selectSql);
    
    // Iterate through the result set and print the attributes.
    while (resultSet.next()) {
        System.out.println(resultSet.getString(2) + " "
            + resultSet.getString(3));
    }

 