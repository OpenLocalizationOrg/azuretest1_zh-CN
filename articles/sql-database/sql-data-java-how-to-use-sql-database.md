---
ms.openlocfilehash: bfb3507454622bd23b50e46f4110ef93719371d0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 SQL Azure (Java) |Microsoft Azure" 
    description="了解如何使用 Azure SQL 数据库从 Java 代码。" 
    services="sql-database" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="jeffreyg" 
    editor="jimbe"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="06/03/2015" 
    ms.author="robmcm"/>

# 如何使用 Java 中的 SQL Azure 数据库

以下步骤显示了如何使用 Java 的 SQL Azure 数据库。 命令行示例显示为简单起见，但是非常相似的步骤将是适合于 web 应用程序，或者托管后端，在 Azure，或在其他环境中。 该指南涵盖了创建服务器和从[Azure 管理门户](https://windows.azure.com)中创建数据库。

## SQL Azure 数据库是什么

Azure 的 SQL 数据库为 Azure，提供了关系数据库管理系统和基于 SQL Server 技术。 与 SQL 数据库实例，您可以轻松地调配和将关系数据库解决方案部署到云，并利用的分布式的数据中心，为企业级的可用性、 可扩展性和安全性提供了内置的数据保护和自我修复的优势。



## 概念
由于 SQL Server 技术内置 SQL Azure 数据库，从 Java 访问 SQL 数据库是非常类似于从 Java 访问 SQL Server。 您可以开发本地应用程序 （使用 SQL Server），然后通过更改仅连接字符串连接到 SQL 数据库。 您的应用程序，可以使用 SQL Server JDBC 驱动程序。 但是，有一些 SQL 数据库和 SQL Server 可能会影响您的应用程序之间的差异。 有关详细信息，请参阅[准则和限制 （SQL 数据库）](http://msdn.microsoft.com/library/windowsazure/ff394102.aspx)。

对于 SQL 数据库的其他资源，请参阅[后续步骤][]部分。

## 先决条件

以下是如果您打算使用 Java 的 SQL 数据库的系统必备组件。

* Java 开发人员工具箱 (JDK)，v 1.6 或更高版本。
* 还可以从<http://www.microsoft.com/windowsazure/offers/>获得 Azure 订阅。
* 如果您正在使用 Eclipse，需要 Eclipse IDE 对于 Java EE 开发，靛蓝色或更高版本。 这可以从<http://www.eclipse.org/downloads/>下载。 此外需要为 Eclipse （通过开放的 Microsoft 技术） 的 Java 与 Azure 插件。 在安装此插件，确保 Microsoft 针对 SQL Server 的 JDBC 驱动程序 4.0 包括。 有关详细信息，请参阅[安装 Azure 的日蚀式 （通过开放的 Microsoft 技术） 的 Java 插件](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx)。
* 如果您没有使用 Eclipse，则需要 Microsoft JDBC 驱动程序 4.0 为 SQL Server，则可以从<http://www.microsoft.com/download/details.aspx?id=11774>下载。

## 创建 SQL Azure 数据库

之前在 Java 代码中使用 SQL Azure 数据库，您将需要创建 SQL Azure 数据库服务器。

1. 登录到[Azure 的管理门户](https://manage.windowsazure.com)。
2. 单击**新建**。

    ![创建新的 SQL 数据库][create_new]

3. 单击**SQL 数据库**，然后单击**创建自定义**。

    ![创建自定义 SQL 数据库][create_new_sql_db]

4. 在**数据库设置**对话框中，指定数据库的名称。 对于本指南的目的，将**gettingstarted**用作数据库的名称。
5. 对于**服务器**，请选择**新的 SQL 数据库服务器**。 使用的其他字段的默认值。

    ![SQL 数据库设置][create_database_settings]

6. 单击下箭头。    
7. 在**服务器设置**对话框中，指定一个 SQL Server 登录名。 对于本指南的目的，使用了**MySQLAdmin** 。 指定并确认密码。 指定一个区域，并确保已选中**允许 Azure 服务来访问服务器**。

    ![SQL 服务器设置][create_server_settings]

8. 单击完成按钮。

## 确定 SQL 数据库的连接字符串

1. 登录到[Azure 的管理门户](https://manage.windowsazure.com)。
2. 单击**SQL 数据库**。
3. 单击您要使用的数据库。
4. 单击**显示的连接字符串**。
5. 突出显示的**JDBC**连接字符串的内容。

    ![确定 JDBC 连接字符串][get_jdbc_connection_string]

6. 用鼠标右键单击突出显示的**JDBC**连接字符串的内容，然后单击**复制**。
7. 您现在可以将此值粘贴到代码文件中，创建以下形式的连接字符串。 *您的服务器*（在两个位置） 替换为您在上一步中复制的文本并将*your_password*替换创建 SQL 数据库帐户时指定的密码的值。 (赋予的值来替换**数据库 =**和**用户 =**如果您没有使用**gettingstarted**和**MySQLAdmin**，分别。) 

    字符串连接 ="jdbc:sqlserver: / /*您的服务器*。 database.windows.net:1433"";"+  
        "数据库 = gettingstarted"+";"+"用户 = MySQLAdmin @*您的服务器*"";"+  
        "密码 =*your_password*"";"+  
        "加密 = true"+";"+"hostNameInCertificate = *.int.mscds.com"";"+  
        "loginTimeout = 30";

我们现在告诉您的步骤，以确定连接字符串实际上使用此字符串在本指南的后面部分。 此外，具体取决于应用程序的需要，您可能不需要使用**加密**和**hostNameInCertificate**的设置，并可能需要修改的**loginTimeout**设置。

## 若要允许访问的 IP 地址范围

1. 登录到[管理门户](https://manage.windowsazure.com)。
2. 单击**SQL 数据库**。
3. 单击**服务器**。
4. 单击您要使用的服务器。
5. 单击**管理**。
6. 单击**配置**。
7. 在**允许的 IP 地址**，输入新的 IP 规则的名称。 指定的开始和结束 IP 地址的范围。 为方便起见，会显示当前的客户端 IP 地址。 下面的示例允许在单个客户端 IP 地址 （IP 地址可能会有所不同）。

    ![允许 IP 地址对话框][allowed_ips_dialog]

8. 单击完成按钮。 现在将允许您指定的 IP 地址访问数据库服务器。

## 若要使用 Azure 在 Java 中的 SQL 数据库

1. 创建一个 Java 项目。 出于本教程的目的，称之为**HelloSQLAzure**。
2. 添加名为**HelloSQLAzure.java** ，对项目的 Java 类文件。
3. 将**Microsoft SQL Server JDBC 驱动程序**添加到生成路径。

   如果您正在使用 Eclipse:

    1. 日蚀式的项目资源管理器中右击**HelloSQLAzure**项目，然后单击**属性**。
    2. 在**属性**对话框的左侧窗格中，单击**Java 构建路径**。
    3. 单击**库**选项卡，然后单击**添加库**。
    4. 在**添加库**对话框中，选择**SQL Server 的 Microsoft JDBC 驱动程序 4.0**，单击**下一步**，然后单击**完成**。
    5. 单击**确定**以关闭**属性**对话框。

    如果您没有使用 Eclipse，Microsoft JDBC 驱动程序 4.0 为添加 SQL Server jar/文件夹到您的类路径。 有关相关信息，请参阅[使用 JDBC 驱动程序](http://msdn.microsoft.com/library/ms378526.aspx)。

4. 在**HelloSQLAzure.java**代码中，添加在`import`语句如下所示︰

        import java.sql.*;
        import com.microsoft.sqlserver.jdbc.*;

5. 指定连接字符串。 下面是一个示例。 如上所述，使用适合于您的 SQL 数据库服务器的值替换*您的服务器*（在两个位置）、 *your_user*和*your_password* 。

        String connectionString =
            "jdbc:sqlserver://your_server.database.windows.net:1433" + ";" +  
                "database=master" + ";" + 
                "user=your_user@your_server" + ";" +  
                "password=your_password";

您现在可以添加将与您的 SQL 数据库服务器进行通信的代码中。

## 从您的代码与 SQL Azure 数据库通信

本主题的其余部分显示了示例，请执行下列操作︰

1. 连接到 SQL 数据库服务器。
2. 定义 SQL 语句，例如，若要创建或删除表、 插入、 选择或删除行等。
3. 执行 SQL 语句，可以通过调用**executeUpdate**或**executeQuery**。
4. 如果适用，显示查询结果。

以下各节用于读取顺序 （取样）。 第一段是一个完整的样本;其他人会依赖在完整的示例，如**导**入语句、**类**和**主**声明、 错误处理和资源结算框架的一部分。

## 若要创建表

下面的代码演示了如何创建一个名为**人**表。

    import java.sql.*;
    import com.microsoft.sqlserver.jdbc.*;
    
    public class HelloSQLAzure {
    
        public static void main(String[] args) 
        {
    
            // Connection string for your SQL Database server.
            // Change the values assigned to your_server, 
            // your_user@your_server,
            // and your_password.
            String connectionString = 
                "jdbc:sqlserver://your_server.database.windows.net:1433" + ";" +  
                    "database=gettingstarted" + ";" + 
                    "user=your_user@your_server" + ";" +  
                    "password=your_password";
            
            // The types for the following variables are
            // defined in the java.sql library.
            Connection connection = null;  // For making the connection
            Statement statement = null;    // For the SQL statement
            ResultSet resultSet = null;    // For the result set, if applicable
            
            try
            {
                // Ensure the SQL Server driver class is available.
                Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
            
                // Establish the connection.
                connection = DriverManager.getConnection(connectionString);
            
                // Define the SQL string.
                String sqlString = 
                    "CREATE TABLE Person (" + 
                        "[PersonID] [int] IDENTITY(1,1) NOT NULL," +
                        "[LastName] [nvarchar](50) NOT NULL," + 
                        "[FirstName] [nvarchar](50) NOT NULL)";
            
                // Use the connection to create the SQL statement.
                statement = connection.createStatement();
            
                // Execute the statement.
                statement.executeUpdate(sqlString);
            
                // Provide a message when processing is complete.
                System.out.println("Processing complete.");
            
            }
            // Exception handling
            catch (ClassNotFoundException cnfe)  
            {
                
                System.out.println("ClassNotFoundException " +
                                   cnfe.getMessage());
            }
            catch (Exception e)
            {
                System.out.println("Exception " + e.getMessage());
                e.printStackTrace();
            }
            finally
            {
                try
                {
                    // Close resources.
                    if (null != connection) connection.close();
                    if (null != statement) statement.close();
                    if (null != resultSet) resultSet.close();
                }
                catch (SQLException sqlException)
                {
                    // No additional action if close() statements fail.
                }
            }
            
        }
    
    }
    

## 在一个表上创建索引

下面的代码演示如何创建名为**index1** **人**表，使用**过程**列上的索引。

    // Connection string for your SQL Database server.
    // Change the values assigned to your_server, 
    // your_user@your_server,
    // and your_password.
    String connectionString = 
        "jdbc:sqlserver://your_server.database.windows.net:1433" + ";" +  
            "database=gettingstarted" + ";" + 
            "user=your_user@your_server" + ";" +  
            "password=your_password";
    
    // The types for the following variables are
    // defined in the java.sql library.
    Connection connection = null;  // For making the connection
    Statement statement = null;    // For the SQL statement
    ResultSet resultSet = null;    // For the result set, if applicable
    
    try
    {
        // Ensure the SQL Server driver class is available.
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    
        // Establish the connection.
        connection = DriverManager.getConnection(connectionString);
    
        // Define the SQL string.
        String sqlString = 
            "CREATE CLUSTERED INDEX index1 " + "ON Person (PersonID)";
    
        // Use the connection to create the SQL statement.
        statement = connection.createStatement();
    
        // Execute the statement.
        statement.executeUpdate(sqlString);
    
        // Provide a message when processing is complete.
        System.out.println("Processing complete.");
    
    }
    // Exception handling and resource closing not shown...



## 插入行

下面的代码演示如何向**人员**表中添加行。

    // Connection string for your SQL Database server.
    // Change the values assigned to your_server, 
    // your_user@your_server,
    // and your_password.
    String connectionString = 
        "jdbc:sqlserver://your_server.database.windows.net:1433" + ";" +  
            "database=gettingstarted" + ";" + 
            "user=your_user@your_server" + ";" +  
            "password=your_password";
    
    // The types for the following variables are
    // defined in the java.sql library.
    Connection connection = null;  // For making the connection
    Statement statement = null;    // For the SQL statement
    ResultSet resultSet = null;    // For the result set, if applicable
    
    try
    {
        // Ensure the SQL Server driver class is available.
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    
        // Establish the connection.
        connection = DriverManager.getConnection(connectionString);
    
        // Define the SQL string.
        String sqlString = 
            "SET IDENTITY_INSERT Person ON " + 
                "INSERT INTO Person " + 
                "(PersonID, LastName, FirstName) " + 
                "VALUES(1, 'Abercrombie', 'Kim')," + 
                      "(2, 'Goeschl', 'Gerhard')," + 
                      "(3, 'Grachev', 'Nikolay')," + 
                      "(4, 'Yee', 'Tai')," + 
                      "(5, 'Wilson', 'Jim')";
    
        // Use the connection to create the SQL statement.
        statement = connection.createStatement();
    
        // Execute the statement.
        statement.executeUpdate(sqlString);
    
        // Provide a message when processing is complete.
        System.out.println("Processing complete.");
    
    }
    // Exception handling and resource closing not shown...

 
## 若要检索行

下面的代码演示如何从**用户**表中检索行。

    // Connection string for your SQL Database server.
    // Change the values assigned to your_server, 
    // your_user@your_server,
    // and your_password.
    String connectionString = 
        "jdbc:sqlserver://your_server.database.windows.net:1433" + ";" +  
            "database=gettingstarted" + ";" + 
            "user=your_user@your_server" + ";" +  
            "password=your_password";
    
    // The types for the following variables are
    // defined in the java.sql library.
    Connection connection = null;  // For making the connection
    Statement statement = null;    // For the SQL statement
    ResultSet resultSet = null;    // For the result set, if applicable
    
    try
    {
        // Ensure the SQL Server driver class is available.
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    
        // Establish the connection.
        connection = DriverManager.getConnection(connectionString);
    
        // Define the SQL string.
        String sqlString = "SELECT TOP 10 * FROM Person";
    
        // Use the connection to create the SQL statement.
        statement = connection.createStatement();
    
        // Execute the statement.
        resultSet = statement.executeQuery(sqlString);
    
        // Loop through the results
        while (resultSet.next())
        {
            // Print out the row data
            System.out.println(
                "Person with ID " + 
                resultSet.getString("PersonID") + 
                " has name " +
                resultSet.getString("FirstName") + " " +
                resultSet.getString("LastName"));
            }
    
        // Provide a message when processing is complete.
        System.out.println("Processing complete.");
    
    }
    // Exception handling and resource closing not shown...

 上面的代码从**用户**表中选择的前 10 行。 如果您想要返回的所有行，则修改下面的 SQL 语句︰

    String sqlString = "SELECT * FROM Person";

 
## 若要检索使用 WHERE 子句的行

若要检索行使用子句，使用代码，如上所示，除了更改包含子句的 SQL 语句。 下面的 SQL 语句包含行其**名字字段**值等于**吉姆**一个子句。

    // Define the SQL string.
    String sqlString = "SELECT * FROM Person WHERE FirstName='Jim'";
    
其中子句还可以使用检索计数，行，在更新或删除行时。

## 若要检索的行数

下面的代码演示如何从**用户**表中检索的行的计数。
 
    // Connection string for your SQL Database server.
    // Change the values assigned to your_server, 
    // your_user@your_server,
    // and your_password.
    String connectionString = 
        "jdbc:sqlserver://your_server.database.windows.net:1433" + ";" +  
            "database=gettingstarted" + ";" + 
            "user=your_user@your_server" + ";" +  
            "password=your_password";
    
    // The types for the following variables are
    // defined in the java.sql library.
    Connection connection = null;  // For making the connection
    Statement statement = null;    // For the SQL statement
    ResultSet resultSet = null;    // For the result set, if applicable
    
    try
    {
        // Ensure the SQL Server driver class is available.
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    
        // Establish the connection.
        connection = DriverManager.getConnection(connectionString);
    
    // Define the SQL string.
        String sqlString = "SELECT COUNT (PersonID) FROM Person";
    
        // Use the connection to create the SQL statement.
        statement = connection.createStatement();
    
        // Execute the statement.
        resultSet = statement.executeQuery(sqlString);
    
        // Print out the returned number of rows.
        while (resultSet.next())
        {
            System.out.println("There were " + 
                             resultSet.getInt(1) +
                             " rows returned.");
        }
    
        // Provide a message when processing is complete.
        System.out.println("Processing complete.");
    
    }
    // Exception handling and resource closing not shown...

## 若要更新的行

下面的代码演示如何更新的行。 在此示例中，**姓氏**值变为任何行，其中的**名字字段**值是**Jim** **Kim** 。

    // Connection string for your SQL Database server.
    // Change the values assigned to your_server, 
    // your_user@your_server,
    // and your_password.
    String connectionString = 
        "jdbc:sqlserver://your_server.database.windows.net:1433" + ";" +  
            "database=gettingstarted" + ";" + 
            "user=your_user@your_server" + ";" +  
            "password=your_password";
    
    // The types for the following variables are
    // defined in the java.sql library.
    Connection connection = null;  // For making the connection
    Statement statement = null;    // For the SQL statement
    ResultSet resultSet = null;    // For the result set, if applicable
    
    try
    {
        // Ensure the SQL Server driver class is available.
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    
        // Establish the connection.
        connection = DriverManager.getConnection(connectionString);
    
        // Define the SQL string.
        String sqlString = 
            "UPDATE Person " + "SET LastName = 'Kim' " + "WHERE FirstName='Jim'";
    
        // Use the connection to create the SQL statement.
        statement = connection.createStatement();
        
        // Execute the statement.
        statement.executeUpdate(sqlString);
    
        // Provide a message when processing is complete.
        System.out.println("Processing complete.");
    
    }// Exception handling and resource closing not shown...

 

## 删除行

下面的代码演示如何删除行。 在此示例中，其中的**名字字段**值是**Jim**任何行将被删除。

    // Connection string for your SQL Database server.
    // Change the values assigned to your_server, 
    // your_user@your_server,
    // and your_password.
    String connectionString = 
        "jdbc:sqlserver://your_server.database.windows.net:1433" + ";" +  
            "database=gettingstarted" + ";" + 
            "user=your_user@your_server" + ";" +  
            "password=your_password";
    
    // The types for the following variables are
    // defined in the java.sql library.
    Connection connection = null;  // For making the connection
    Statement statement = null;    // For the SQL statement
    ResultSet resultSet = null;    // For the result set, if applicable
    
    try
    {
        // Ensure the SQL Server driver class is available.
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    
        // Establish the connection.
        connection = DriverManager.getConnection(connectionString);
    
        // Define the SQL string.
        String sqlString = 
            "DELETE from Person " + 
                "WHERE FirstName='Jim'";
    
        // Use the connection to create the SQL statement.
        statement = connection.createStatement();
    
        // Execute the statement.
        statement.executeUpdate(sqlString);
    
        // Provide a message when processing is complete.
        System.out.println("Processing complete.");
    
    }
    // Exception handling and resource closing not shown...
    
 
## 若要检查是否存在表

下面的代码演示了如何确定表是否存在。

    // Connection string for your SQL Database server.
    // Change the values assigned to your_server, 
    // your_user@your_server,
    // and your_password.
    String connectionString = 
        "jdbc:sqlserver://your_server.database.windows.net:1433" + ";" +  
            "database=gettingstarted" + ";" + 
            "user=your_user@your_server" + ";" +  
            "password=your_password";
    
    // The types for the following variables are
    // defined in the java.sql library.
    Connection connection = null;  // For making the connection
    Statement statement = null;    // For the SQL statement
    ResultSet resultSet = null;    // For the result set, if applicable
    
    try
    {
        // Ensure the SQL Server driver class is available.
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    
        // Establish the connection.
        connection = DriverManager.getConnection(connectionString);
    
        // Define the SQL string.
        String sqlString = 
            "IF EXISTS (SELECT 1 " +
                "FROM sysobjects " + 
                "WHERE xtype='u' AND name='Person') " +
                "SELECT 'Person table exists.'" +
                "ELSE  " +
                "SELECT 'Person table does not exist.'";
    
        // Use the connection to create the SQL statement.
        statement = connection.createStatement();
    
        // Execute the statement.
        resultSet = statement.executeQuery(sqlString);
    
        // Display the result.
        while (resultSet.next())
        {
            System.out.println(resultSet.getString(1));
        }
    
        // Provide a message when processing is complete.
        System.out.println("Processing complete.");
    
    }
    // Exception handling and resource closing not shown...

## 若要删除索引

下面的代码演示如何删除名为**index1** **人**表上的索引。

    // Connection string for your SQL Database server.
    // Change the values assigned to your_server, 
    // your_user@your_server,
    // and your_password.
    String connectionString = 
        "jdbc:sqlserver://your_server.database.windows.net:1433" + ";" +
            "database=gettingstarted" + ";" +
            "user=your_user@your_server" + ";" +
            "password=your_password";
    
    // The types for the following variables are
    // defined in the java.sql library.
    Connection connection = null;  // For making the connection
    Statement statement = null;    // For the SQL statement
    ResultSet resultSet = null;    // For the result set, if applicable
    
    try
    {
        // Ensure the SQL Server driver class is available.
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    
        // Establish the connection.
        connection = DriverManager.getConnection(connectionString);
    
        // Define the SQL string.
        String sqlString = 
            "DROP INDEX index1 " + 
                "ON Person";
    
        // Use the connection to create the SQL statement.
        statement = connection.createStatement();
    
        // Execute the statement.
        statement.executeUpdate(sqlString);
    
        // Provide a message when processing is complete.
        System.out.println("Processing complete.");
    
    }
    // Exception handling and resource closing not shown...

 
## 若要删除的表

下面的代码演示如何删除名为**人**表。

    // Connection string for your SQL Database server.
    // Change the values assigned to your_server, 
    // your_user@your_server,
    // and your_password.
    String connectionString = 
        "jdbc:sqlserver://your_server.database.windows.net:1433" + ";" +  
            "database=gettingstarted" + ";" + 
            "user=your_user@your_server" + ";" +  
            "password=your_password";
    
    // The types for the following variables are
    // defined in the java.sql library.
    Connection connection = null;  // For making the connection
    Statement statement = null;    // For the SQL statement
    ResultSet resultSet = null;    // For the result set, if applicable
    
    try
    {
        // Ensure the SQL Server driver class is available.
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    
        // Establish the connection.
        connection = DriverManager.getConnection(connectionString);
    
        // Define the SQL string.
        String sqlString = "DROP TABLE Person";
    
        // Use the connection to create the SQL statement.
        statement = connection.createStatement();
    
        // Execute the statement.
        statement.executeUpdate(sqlString);
    
        // Provide a message when processing is complete.
        System.out.println("Processing complete.");
    
    }
    // Exception handling and resource closing not shown...

## 使用 SQL Azure 的部署中的 Java 中的数据库

若要在 Azure 的部署中，除了作为库在您的类路径，如上所示的 SQL Server 具有 Microsoft JDBC 驱动程序 4.0 中使用 Java 中的 SQL 数据库将需要将它打包到部署。


**如果您正在使用 Eclipse，打包 Microsoft JDBC 驱动程序 4.0 SQL Server**

1. 日蚀式的项目资源管理器中右击项目并单击**属性**。
2. 在**属性**对话框的左侧窗格中，单击**部署程序集**，，然后单击**添加**。
3. 在**新程序集指令**对话框中，单击**Java 构建路径条目**，然后单击**下一步**。
4. 选择**Microsoft JDBC 驱动程序 4.0 SQL Server** ，然后单击**完成**。
5. 单击**确定**以关闭**属性**对话框。
6. 将项目的 WAR 文件导出到 approot 文件夹中，并重建 Azure 项目中，每个记录创建[一个你好世界上应用程序使用 Azure 的日蚀式 （通过开放的 Microsoft 技术） 的 Java 插件](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx)的步骤。 该主题还描述如何在计算仿真程序，并在 Azure 中运行您的应用程序。

**如果您没有使用 Eclipse，打包 Microsoft JDBC 驱动程序 4.0 SQL Server**

* 请确保 Microsoft JDBC 驱动程序 4.0 SQL Server 库是包含在相同的 Azure 角色作为 Java 应用程序，并添加到您的应用程序的类路径。

## 下一步行动

要为 SQL Server 中了解有关 Microsoft JDBC 驱动程序的详细信息，请参阅[概述 JDBC 驱动程序](http://msdn.microsoft.com/library/ms378749.aspx)。 若要了解有关 SQL 数据库的详细信息，请参阅[SQL 数据库概述](http://msdn.microsoft.com/library/windowsazure/ee336241.aspx)。

[概念]:#concepts
[先决条件]:#prerequisites
[创建 SQL Azure 数据库]:#create_db
[确定 SQL 数据库的连接字符串]:#determine_connection_string
[若要允许访问的 IP 地址范围]:#specify_allowed_ips
[若要使用 Azure 在 Java 中的 SQL 数据库]:#use_sql_azure_in_java
[从您的代码与 SQL Azure 数据库通信]:#communicate_from_code
[若要创建表]:#to_create_table
[在一个表上创建索引]:#to_create_index
[插入行]:#to_insert_rows
[若要检索行]:#to_retrieve_rows
[若要检索使用 WHERE 子句的行]:#to_retrieve_rows_using_where
[若要检索的行数]:#to_retrieve_row_count
[若要更新的行]:#to_update_rows
[删除行]:#to_delete_rows
[若要检查是否存在表]:#to_check_table_existence
[若要删除索引]:#to_drop_index
[若要删除的表]:#to_drop_table
[使用 SQL Azure 的部署中的 Java 中的数据库]:#using_in_azure
[下一步行动]:#nextsteps
[create_new]: ./media/sql-data-java-how-to-use-sql-database/WA_New.png
[create_new_sql_db]: ./media/sql-data-java-how-to-use-sql-database/WA_SQL_DB_Create.png
[create_database_settings]: ./media/sql-data-java-how-to-use-sql-database/WA_CustomCreate_1.png
[create_server_settings]: ./media/sql-data-java-how-to-use-sql-database/WA_CustomCreate_2.png
[get_jdbc_connection_string]: ./media/sql-data-java-how-to-use-sql-database/WA_SQL_JDBC_ConnectionString.png
[allowed_ips_dialog]: ./media/sql-data-java-how-to-use-sql-database/WA_Allowed_IPs.png
 
