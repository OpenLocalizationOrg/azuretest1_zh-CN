---
ms.openlocfilehash: 7a4e1c9d51ab1e8677b843cb2688cc43a93d7352
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="代码示例︰ 重试逻辑 C# 连接到 SQL 数据库 |Microsoft Azure" 
    description="C# 示例包含与 SQL Azure 数据库进行交互的功能强大的重试逻辑。" 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jeffreyg" 
    editor=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="genemi"/>


# 代码示例︰ 重试连接到 SQL 数据库中 C# 的逻辑


<!--
sql-database-develop-csharp-retry-windows.md  ,  NEW
mgm4f20150723retryfrommsdn68
. . . .
Old Titles on MSDN:  Code sample: Retry logic for connecting to Azure SQL Database with ADO.NET
.
How to: Connect to Azure SQL Database by using ADO.NET
.
ShortId on MSDN was:  ee336243.aspx
Dx  4d7936fd-341c-4a37-8796-25e385ae6c5b
-->


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


本主题提供的 C# 代码示例，演示如何自定义重试逻辑。 重试逻辑旨在妥善处理临时错误或*瞬时故障*，往往会消失，如果程序将等待几秒钟再重试。


您用来连接到您的本地 Microsoft SQL Server 的 ADO.NET 类还可以连接到 SQL Azure 数据库。
但是，本身 ADO.NET 类不能提供所有的稳定性和可靠性有必要在生产中使用。
客户端程序可能会遇到暂时性错误，从中它应以无提示方式和正常恢复自己。 瞬时故障的一些示例︰


- 通过互联网连接受到短暂的网络中断，其后可以重新创建连接。

- 云计算涉及到负载平衡可以暂时阻止试图连接或查询。


## 确定瞬态错误


您的程序必须区分瞬态错误，而不是永久的错误。
如果您的程序具有目标数据库名称的拼写错误，"没有这样的数据库找到"错误将保持不变，每次您重新运行该程序时将会再次发生。


查看器分为瞬时故障的错误号的列表︰


- [对于 SQL 数据库的客户端程序的错误消息](sql-database-develop-error-messages.md#bkmk_connection_errors)
 - 请参阅有关*瞬态错误、 连接丢失错误*部分。


## C# 代码示例


当前主题中的 C# 代码示例包含自定义检测和重试逻辑，用于处理瞬态错误。


该代码示例遵循一些基本原则或适用无论哪一种技术建议使用与 SQL Azure 数据库进行交互。
您可以看到在一般性建议︰


- [连接到 SQL 数据库︰ 链接、 最佳做法和设计指南](sql-database-connect-central-recommendations.md)


C# 代码示例包含两个.cs 文件的内容粘贴到以下各节。  其文件名为︰


- `Program.cs`
- `Custom_SqlDatabaseTransientErrorDetectionStrategy.cs`


## 编译和运行代码示例


您可以编译该示例使用以下步骤︰


1. 在 Visual Studio，从 C# 控制台应用程序模板创建新项目。

2. 用鼠标右键单击您的项目，并在本主题中添加的.cs 代码提供的源。

3. 在`cmd.exe`命令窗口中，如下所示运行该程序。 此外显示了运行中的实际输出。


        [C:\MyVS\ConsoleApplication1\ConsoleApplication1\bin\Debug\]
        >> ConsoleApplication1.exe
        database_firewall_rules_table   245575913
        filestream_tombstone_2073058421 2073058421
        filetable_updates_2105058535    2105058535
        
        [C:\MyVS\ConsoleApplication1\ConsoleApplication1\bin\Debug\]
        >>


在下面的部分是 C# 源代码的.cs 文件。


## Program.cs 文件


设计的演示程序，以便︰


- 在试图连接的过程中的瞬态故障会导致重试。

- 查询命令期间暂时的错误会导致程序放弃连接并创建一个新的连接之前重试查询命令。


Microsoft 不建议也不不同建议本设计选择。
演示程序演示了一些可供您的设计灵活性。


### 源代码的长度


长`Program.cs`文件是主要是因为捕获异常的逻辑。


[较短的版本](#shorter_program_cs)之一`Program.cs`文件显示在本主题中，所有的重试逻辑末尾和`Exception`删除处理。


### 调用堆栈


`Main`方法是在`Program.cs`。 调用堆栈运行，如下所示︰


1. `Main` 调用`ConnectAndQuery`。

2. `ConnectAndQuery` 调用`EstablishConnection`。

3. `EstablishConnection` 调用`IssueQueryCommand`。


### Program.cs 的源代码


    using     System;  // C#, pure ADO.NET, no Enterprise Library.
    using X = System.Text;
    using D = System.Data;
    using C = System.Data.SqlClient;
    using T = System.Threading;
    
    namespace ConsoleApplication1
    {
        class Program
        {
            // Fields, shared among methods.
            C.SqlConnection sqlConnection;
            C.SqlConnectionStringBuilder scsBuilder;
    
            static void Main(string[] args)
            {
                new Program().ConnectAndQuery();
            }
    
            /// <summary>
            /// Prepares values for a connection. Then inside a loop, it calls a method
            /// that opens a connection. The called method calls yet another method
            /// that issues a query.
            /// The loop reiterates only if a transient error is encountered.
            /// </summary>
            void ConnectAndQuery()
            {
                int connectionTimeoutSeconds = 30;  // Default of 15 seconds is too short over the Internet, sometimes.
                int maxCountTriesConnectAndQuery = 3;  // You can adjust the various retry count values.
                int secondsBetweenRetries = 6;  // Simple retry strategy.
    
                // [A.1] Prepare the connection string to Azure SQL Database.
                this.scsBuilder = new C.SqlConnectionStringBuilder();
                // Change these values to your values.
                this.scsBuilder["Server"] = "tcp:myazuresqldbserver.database.windows.net,1433";
                this.scsBuilder["User ID"] = "MyLogin";  // @yourservername suffix sometimes.
                this.scsBuilder["Password"] = "MyPassword";
                this.scsBuilder["Database"] = "MyDatabase";
                // Leave these values as they are.
                this.scsBuilder["Trusted_Connection"] = false;
                this.scsBuilder["Integrated Security"] = false;
                this.scsBuilder["Encrypt"] = true;
                this.scsBuilder["Connection Timeout"] = connectionTimeoutSeconds;
    
                //-------------------------------------------------------
                // Preparations are complete.
    
                for (int cc = 1; cc <= maxCountTriesConnectAndQuery; cc++)
                {
                    try
                    {
                        // [A.2] Connect, which proceeds to issue a query command.
                        this.EstablishConnection();
    
                        // [A.3] All has gone well, so let the program end.
                        break;
                    }
                    catch (C.SqlException sqlExc)
                    {
                        bool isTransientError;
    
                        // [A.4] Check whether sqlExc.Number is on the whitelist of transients.
                        isTransientError = Custom_SqlDatabaseTransientErrorDetectionStrategy
                            .IsTransientStatic(sqlExc);
    
                        if (isTransientError == false)  // Is a persistent error...
                        {
                            Console.WriteLine();
                            Console.WriteLine("Persistent error suffered, SqlException.Number=={0}.  Will terminate.",
                                sqlExc.Number);
                            Console.WriteLine(sqlExc.ToString());
    
                            // [A.5] Either the connection attempt or the query command attempt suffered a persistent SqlException.
                            // Break the loop, let the hopeless program end.
                            break;
                        }
    
                        // [A.6] The SqlException identified a transient fault from an attempt to issue a query command.
                        // So let this method reloop and try again. However, we recommend that the new query
                        // attempt should start at the beginning and establish a new connection.
                        Console.WriteLine();
                        Console.WriteLine("Transient error encountered.  SqlException.Number=={0}."
                            + "  Program might retry by itself.", sqlExc.Number);
                        Console.WriteLine("{0} = Attempts so far. Might retry.", cc);
                        Console.WriteLine(sqlExc.Message);
                    }
                    catch (Exception exc)
                    {
                        Console.WriteLine();
                        Console.WriteLine("Unexpected exception type caught in Main. Will terminate.");
    
                        // [A.7] The program must end, so re-throw the unrecognized error.
                        throw exc;
                    }
    
                    // [A.8] Throw an application exception if transient SqlExceptions caused us
                    // to exceed our self-imposed maximum count of retries.
                    if (cc > maxCountTriesConnectAndQuery)
                    {
                        Console.WriteLine();
                        string mesg = String.Format(
                            "Transient errors suffered in too many retries ({0}). Will terminate.",
                            cc - 1);
                        Console.WriteLine(mesg);
    
                        // [A.9] To end the program, throw a new exception of a different type.
                        ApplicationException appExc = new ApplicationException(mesg);
                        throw appExc;
                    }
                    // Else, can retry.
    
                    // A very simple retry strategy, a brief pause before looping.
                    T.Thread.Sleep(1000 * secondsBetweenRetries);
                } // for cc
                return;
            } // method ConnectAndQuery
    
    
            /// <summary>
            /// Open a connection, then call a method that issues a query.
            /// </summary>
            void EstablishConnection()
            {
                try
                {
                    // [B.1] The 'using' statement will .Dispose() the connection.
                    // If you are working with a connection pool, you might want instead
                    // to merely .Close() the connection.
                    using (this.sqlConnection = new C.SqlConnection(this.scsBuilder.ToString()))
                    {
                        // [B.2] Open a connection.
                        sqlConnection.Open();
                        // [B.3]
                        this.IssueQueryCommand();
                    }
                }
                catch (Exception exc)
                {
                    // [B.4] This re-throw means we discard the connection whenever
                    // any error occurs during query command, even for a transient error.
                    throw exc;  // [B.5] Let caller assess any exception, SqlException or any kind.
                }
                return;
            } // method EstablishConnection
    
    
            /// <summary>
            /// Issue a query, then write the result rows to the console.
            /// </summary>
            void IssueQueryCommand()
            {
                D.IDataReader dReader = null;
                D.IDbCommand dbCommand = null;
                X.StringBuilder sBuilder = new X.StringBuilder(512);
    
                try
                {
                    // [C.1] Use the connection to create a query command.
                    using (dbCommand = this.sqlConnection.CreateCommand())
                    {
                        dbCommand.CommandText =
                            @"SELECT TOP 3
                                  ob.name,
                                  CAST(ob.object_id as nvarchar(32)) as [object_id]
                              FROM sys.objects as ob
                              WHERE ob.type='IT'
                              ORDER BY ob.name;";
    
                        // [C.2] Issue the query command through the connection.
                        using (dReader = dbCommand.ExecuteReader())
                        {
                            // [C.3] Loop through all returned rows, writing the data to the console.
                            while (dReader.Read())
                            {
                                sBuilder.Length = 0;
                                sBuilder.Append(dReader.GetString(0));
                                sBuilder.Append("\t");
                                sBuilder.Append(dReader.GetString(1));
    
                                Console.WriteLine(sBuilder.ToString());
                            }
                        }
                    }
                }
                catch (Exception exc)
                {
                    throw exc; // Let caller assess any exception.
                }
                return;
            } // method IssueQueryCommand
        } // class Program
    }


## Custom_SqlDatabaseTransientErrorDetectionStrategy.cs 代码文件


[`SqlDatabaseTransientErrorDetectionStrategy`](http://msdn.microsoft.com/library/microsoft.practices.enterpriselibrary.transientfaulthandling.sqldatabasetransienterrordetectionstrategy.aspx)是企业库 (EntLib) API 中的类。
存在的代码示例使用以下借用这一概念的 EntLib 类的自定义类。


    using     System;
    using G = System.Collections.Generic;
    using C = System.Data.SqlClient;
    
    namespace ConsoleApplication1
    {
        /// <summary>
        /// A custom alternative to class SqlDatabaeTransientErrorDetectionStrategy.
        /// </summary>
        public class Custom_SqlDatabaseTransientErrorDetectionStrategy
        {
            static private G.List<int> M_listTransientErrorNumbers;
    
    
            /// <summary>
            /// This method happens to match ITransientErrorDetectionStrategy of EntLib60.
            /// </summary>
            public bool IsTransient(Exception exc)
            {
                return IsTransientStatic(exc);
            }
    
    
            /// <summary>
            /// For general use beyond formal Enterprise Library classes.
            /// </summary>
            static public bool IsTransientStatic(Exception exc)
            {
                bool returnBool = false;  // Assume is a persistent error.
                C.SqlException sqlExc;
    
                if (exc is C.SqlException)
                {
                    sqlExc = exc as C.SqlException;
                    if (M_listTransientErrorNumbers.Contains(sqlExc.Number) == true)
                    {
                        returnBool = true;  // Error is transient, not persistent.
                    }
                }
                return returnBool;
            }
    
    
            /// <summary>
            /// Lists the SqlException.Number values that are considered
            /// to indicate transient errors.
            /// </summary>
            static Custom_SqlDatabaseTransientErrorDetectionStrategy()
            {
                int[] arrayOfTransientErrorNumbers =
                    {4060, 10928, 10929, 40197, 40501, 40613 };
    
                M_listTransientErrorNumbers = new G.List<int>(arrayOfTransientErrorNumbers);
            }
        } // class Custom_SqlDatabaseTransientErrorDetectionStrategy
    }


<a id="shorter_program_cs" name="shorter_program_cs"></a>


&nbsp;


## 短的 Program.cs


这一节中的源代码是缩短的重复的长`Program.cs`文件前面介绍。
已删除所有重试逻辑和所有异常处理。


短便于您查看 ADO.NET 调用，知道这些通常工作。
通常没有瞬时性故障发生，并且不会引发异常。
并且通常 skydiver 不需要备份 parachute。


    using     System;  // C#, pure ADO.NET, no retry logic, no Exception handling.
    using X = System.Text;
    using D = System.Data;
    using C = System.Data.SqlClient;
    
    namespace ConsoleApplication1_dn864744
    {
        class Program
        {
            C.SqlConnection sqlConnection;
            C.SqlConnectionStringBuilder scsBuilder;
    
            static void Main(string[] args)
            {
                new Program().ConnectAndQuery();
            }
    
            void ConnectAndQuery()
            {
                this.scsBuilder = new C.SqlConnectionStringBuilder();
                // Change these values to your values.
                this.scsBuilder["Server"] = "tcp:myazuresqldbserver.database.windows.net,1433";
                this.scsBuilder["User ID"] = "MyLogin";
                this.scsBuilder["Password"] = "MyPassword";
                this.scsBuilder["Database"] = "MyDatabase";
                this.scsBuilder["Trusted_Connection"] = false;
                this.scsBuilder["Integrated Security"] = false;
                this.scsBuilder["Encrypt"] = true;
                this.scsBuilder["Connection Timeout"] = 30;
    
                this.EstablishConnection();
            } // method ConnectAndQuery
    
            void EstablishConnection()
            {
                using (this.sqlConnection = new C.SqlConnection(this.scsBuilder.ToString()))
                {
                    sqlConnection.Open();
                    this.IssueQueryCommand();
                }
            } // method EstablishConnection
    
            void IssueQueryCommand()
            {
                D.IDataReader dReader = null;
                D.IDbCommand dbCommand = null;
                X.StringBuilder sBuilder = new X.StringBuilder(512);
    
                using (dbCommand = this.sqlConnection.CreateCommand())
                {
                    dbCommand.CommandText =
                        @"SELECT TOP 3 ob.name, CAST(ob.object_id as nvarchar(32)) as [object_id]
                            FROM sys.objects as ob
                            WHERE ob.type='IT'
                            ORDER BY ob.name;";
    
                    using (dReader = dbCommand.ExecuteReader())
                    {
                        while (dReader.Read())
                        {
                            sBuilder.Length = 0;
                            sBuilder.Append(dReader.GetString(0));
                            sBuilder.Append("\t");
                            sBuilder.Append(dReader.GetString(1));
                            Console.WriteLine(sBuilder.ToString());
                        }
                    }
                }
            } // method IssueQueryCommand
        } // class Program
    }


## 相关的链接


- [对 SQL 数据库的客户端快速启动代码示例](sql-database-develop-quick-start-client-code-samples.md)

