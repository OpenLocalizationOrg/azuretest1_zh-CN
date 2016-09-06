---
ms.openlocfilehash: 811b703c1cb0375ffa12a8b441fedfa63b502e38
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="连接到并查询您的 SQL 数据库与 C#"
    description="使用 ADO.NET 连接到并与 SQL Azure 数据库云服务上 AdventureWorks 数据库交互的 C# 客户端的代码示例。"
    services="sql-database"
    documentationCenter=""
    authors="ckarst"
    manager="jeffreyg"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article" 
    ms.date="07/17/2015"
    ms.author="cakarst"/>


# 连接到并查询您的 SQL 数据库使用 C & #x 23;


本主题提供的 C# 代码示例，演示如何通过使用 ADO.NET 连接到现有 AdventureWorks SQL 数据库。 控制台应用程序查询数据库并显示结果集编译该示例。


## 先决条件


- 在 SQL Azure 数据库现有 AdventureWorks 数据库。 [创建以分钟为单位](sql-database-get-started.md)。
- [Visual Studio.NET Framework 使用](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)


## 步骤 1︰ 控制台应用程序


1. 创建一个 C# 控制台应用程序使用 Visual Studio。


![连接和查询](./media/sql-database-connect-query/ConnectandQuery_VisualStudio.png)


## 步骤 2: SQL 代码示例


1. 复制并粘贴到控制台应用程序下面的代码示例。


> [AZURE.WARNING] 此代码示例被旨在作为尽可能地短以使其更易于理解。 此示例不打算用于生产中。


这段代码并不适合生产。 如果您想要实现生产就绪的代码，下面被视为业界最佳做法︰


- 异常处理。
- 重试逻辑的瞬态错误。
- 在配置文件中的密码的安全存储。



### C# 示例的源代码


将此代码粘贴到**Program.cs**文件。


    using System;  // C#
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data.SqlClient;

    namespace ConnectandQuery_Example
    {
        class Program
        {
            static void Main()
            {
                string SQLConnectionString = "[Your_Connection_String]";
                // Create a SqlConnection from the provided connection string.
                using (SqlConnection connection = new SqlConnection(SQLConnectionString))
                {
                    // Begin to formulate the command.
                    SqlCommand command = new SqlCommand();
                    command.Connection = connection;

                    // Specify the query to be executed.
                    command.CommandType = System.Data.CommandType.Text;
                        command.CommandText =
                        @"SELECT TOP 10
                        CustomerID, NameStyle, Title, FirstName, LastName
                        FROM SalesLT.Customer";

                    // Open connection to database.
                    connection.Open();

                    // Read data from the query.
                    SqlDataReader reader = command.ExecuteReader();
                    while (reader.Read())
                    {
                        // Formatting will depend on the contents of the query.
                        Console.WriteLine("Value: {0}, {1}, {2}, {3}, {4}",
                            reader[0], reader[1], reader[2], reader[3], reader[4]);
                    }
                }
                Console.WriteLine("Press any key to continue...");
                Console.ReadKey();
            }
        }
    }


## 步骤 3︰ 将连接字符串查找数据库


1. 打开[Azure 预览门户](http://portal.azure.com/)。
2. 单击**浏览** > **SQL 数据库** > **"艾德"数据库**> **属性** > **显示数据库连接字符串**。


![门户](./media/sql-database-connect-query/ConnectandQuery_portal.png)


在数据库连接字符串刀片，ADO.NET、 ODBC，PHP，和 JDBC 找到适当的连接字符串。


## 第 4 步︰ 用来代替真正的连接信息


- 粘贴的源代码，在*[Your_Connection_String]*占位符替换连接字符串，并确保在该字符串中的*your_password_here*替换为您的实际密码。


## 步骤 5︰ 运行应用程序


1. 若要生成并运行您的应用程序，请单击**调试** > **开始调试**
2. 该程序打印到控制台窗口的查询结果。
 