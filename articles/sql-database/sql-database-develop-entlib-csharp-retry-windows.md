---
ms.openlocfilehash: 30a508d14707662246de96de50e01019832ccef3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="代码示例︰ 重试逻辑与企业库，在 C# 为连接到 SQL 数据库 |Microsoft Azure"
    description="企业库旨在减轻包括重试逻辑，为您的客户端程序访问云服务的任务。"
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jeffreyg"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="genemi"/>


# 代码示例︰ 重试逻辑与企业库中，在 C & #x 23;用于连接到 SQL 数据库


本主题提供了完整的代码示例演示企业库 (EntLib)。  EntLib 可以简化程序员的任务包括在他们与 Microsoft Azure SQL 数据库云服务进行交互的客户端的重试逻辑。


企业库 6 (EntLib60) 是最新版本。


深入了解 EntLib 推荐使用下面的链接︰


- [企业库 6--4 月 2013](http://msdn.microsoft.com/library/dn169621.aspx)<br/>提供大量链接的详细信息。

- 中的免费电子书。从 Microsoft 的 PDF 格式︰<br/>[对 Microsoft 企业库，第二版的开发人员指南 》](http://www.microsoft.com/download/details.aspx?id=41145)。


## 先决条件


您在安装 Visual Studio 和 Microsoft.NET Framework 时，不包括 EntLib。 您必须执行一个单独的下载操作。


**NuGet**系统通过 Visual Studio 轻松下载。 在目录中的下载结果命名 Visual Studio 解决方案.sln 文件所在的同一目录下的 packages\。


EntLib 程序集.dll 文件安装子目录中。 两个程序集文件名称如下︰


- `Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.dll`
- `Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.Data.dll`


## C & #x 23;所述的代码文件


C# 代码示例由三个.cs 文件，其内容将被粘贴到下面两节组成。 其文件名为︰


- `Program.cs`
- `Custom_SqlDatabaseTransientErrorDetectionStrategy.cs`
- `ForcePersistent_SqlCommandTransientErrorDetectionStrategy.cs`


### 短`Program.cs`不带 `try/catch`


提供的其他.cs 文件是较短版本的`Program.cs`。 所有`try/catch`代码已被删除。 这使得更易于查看 EntLib 调用的眼睛。



### 测试.cs 文件


测试重试逻辑是棘手的因为没有明显的方法可以导致真正的瞬时性故障错误。 一种测试解决方案是使用临时代码︰


1. 会导致 pretend 的瞬时性故障。
2. 修复的瞬时性故障的原因。
3. 重试连接或查询，应为成功。


为了帮助您测试，另一个.cs 文件，只要是可以用作临时代替︰


- `Test2_TransientErrorDetectionStrategy`
 - 临时替换`Custom_SqlDatabaseTransientErrorDetectionStrategy.cs`。


要更改此测试标记了包含该字符串的注释的代码位置`TEST.PASSWORD_FIX`。


## 编译和运行代码示例


您可以编译该示例使用以下步骤︰


1. 在 Visual Studio，从 C# 控制台应用程序模板创建新项目。

2. 您的项目中，用鼠标右键单击，然后添加本主题中的代码提供哪些源的.cs 文件。

3. 在`cmd.exe`命令窗口中，如下所示运行该程序。 此外显示了运行中的实际输出︰


        [C:\MyVS\ConsoleApplication1\ConsoleApplication1\bin\Debug\]
        >> ConsoleApplication1.exe
        
        database_firewall_rules_table   245575913
        filestream_tombstone_2073058421 2073058421
        filetable_updates_2105058535    2105058535
        
        [C:\MyVS\ConsoleApplication1\ConsoleApplication1\bin\Debug\]
        >>


在下面的部分是 C# 源代码的.cs 文件。


## Program.cs 文件


下面的代码文件，Program.cs，是长的因为它包含`try/catch`仅在发生错误时执行的块。 附近有主题的结尾是短得多版本的`Program.cs`所有`try/catch`删除的行。


`Main`方法是在`Program.cs`。 调用堆栈运行，如下所示︰


1. `Main` 调用`ConnectAndQuery`。

2. `ConnectAndQuery` 调用`InitializeFields`。

3. `ConnectAndQuery` 调用`EstablishConnection`。

4. `EstablishConnection` 调用`IssueQueryCommand`。


&nbsp;


    using     System;  // C#
    using X = System.Text;
    using D = System.Data;
    using C = System.Data.SqlClient;
    using E = Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling;
    
    namespace ConsoleApplication1 
    {
        /// <summary>
        /// Demo sample C# program that uses Microsoft's Enterprise Library 6 to robustly
        /// connect to and interact with Azure SQL Database.
        /// .
        /// The sample chooses a strategy of discarding the Sql Connection whenever
        /// a Sql Command fails, even for a transient reason. The 'for' loop
        /// manages this choice. We are neither recommending nor disrecommending
        /// this particular choice, just demonstrating flexibility.
        /// .
        /// For an optional test mechanism manually built into this program, find all:
        /// 'TEST.PASSWORD_FIX' (related to connection).
        /// </summary>
        class Program
        {
            // const fields, so that SqlException objects thrown from Sql Commands
            // are tested for transience by our custom logic.
            private const string M_progressCreatingConnection = "CreatingConnection";
            private const string M_progressIssuingCommand     = "IssuingCommand";
    
            // Fields, shared among methods.
            private string progressLocation;
            private E.ReliableSqlConnection rsConnection;
            private E.RetryPolicy connectionRetryPolicy;
            private E.RetryPolicy commandRetryPolicy;
            private C.SqlConnectionStringBuilder scsBuilder;
            private Action EstablishConnection_action;
            private Action IssueQueryCommand_action;
            private string sqlSelectCode;
            private ForcePersistent_SqlCommandTransientErrorDetectionStrategy
                forcePersistent_SqlCommandTransientErrorDetectionStrategy;
    
            // Also consider E.FixedInterval or E.Incremental.
            private E.ExponentialBackoff exponentialBackoffStrategy;
    
            // TEST.PASSWORD_FIX.  Replace with Test2_TransientErrorDetectionStrategy.
            private Custom_SqlDatabaseTransientErrorDetectionStrategy
                custom_SqlDatabaseTransientErrorDetectionStrategy;
    
    
            static void Main(string[] args)
            {
                new Program().ConnectAndQuery();
            }
    
    
            /// <summary>
            /// Called from Main.
            /// Calls a method to establish a connection, which calls
            /// another method to issue a query through the connection. 
            /// </summary>
            void ConnectAndQuery()
            {
                int maxCountTriesConnectAndQuery = 3;  // Adjustable.
    
                this.InitializeFields();
    
                //-------------------------------------------------------
                // Preparations are complete.
                // Next is a call .ExecuteAction to invoke the method EstablishConnection.
                // Method EstablishConnection then calls another .ExecuteAction to
                //     invoke the method IssueQueryCommand.
                // Then control is returned to this method.
    
                for (int cc = 1; cc <= maxCountTriesConnectAndQuery; cc++)
                {
                    if (cc >= 2) { Console.WriteLine("-- Another iteration of outer loop, cc=={0}. --", cc); }
    
                    try
                    {
                        // [A.1] Connect, which proceeds to issue a query command.
                        this.connectionRetryPolicy.ExecuteAction(this.EstablishConnection_action);
    
                        break; // [A.2] All has gone well, so let the program end.
                    }
                    catch (C.SqlException sqlExc)
                    {
                        bool isTransientErrorNumber = false;
    
                        // [A.3] Call our collection of transient error numbers.
                        isTransientErrorNumber =
                            Custom_SqlDatabaseTransientErrorDetectionStrategy // TEST.PASSWORD_FIX.
                            .IsTransientStatic(sqlExc);
    
                        if (isTransientErrorNumber == false)
                        {
                            Console.WriteLine();
                            Console.WriteLine("Persistent error suffered, SqlException.Number = {0}."
                                + "  Will terminate.", sqlExc.Number);
                            Console.WriteLine(sqlExc.ToString());
    
                            // [A.4] Either the connection attempt or the query command attempt
                            //     suffered a persistent SqlException.
                            // Break the query command loop, let the hopeless program end.
                            break;
                        }
    
                        if (this.progressLocation == M_progressCreatingConnection)
                        {
                            // [A.5] The EntLib retry mechanisms already gave the Sql Connection
                            //     enough retries, so this block will not give connection any more tries.
                            // This block gives retries to certain Sql Command only.
                            break;
                        }
    
                        // [A.6] The SqlException identified a transient error from an attempt
                        //     to issue a query command.
                        // So let this method reloop and try again. However, we recommend that the new
                        // query attempt should start at the beginning and establish a new connection.
                        Console.WriteLine();
                        Console.WriteLine("Transient error encountered.  SqlException.Number = {0}."
                            + "  Program might retry by itself.", sqlExc.Number);
                        Console.WriteLine("{0} = Attempts so far. Might retry.", cc);
                        Console.WriteLine(sqlExc.Message);
                    }
                    catch (Exception exc)
                    {
                        Console.WriteLine();
                        Console.WriteLine("Unexpected exception type caught.  Will terminate.");
                        throw exc; // [A.7] The program must end, so re-throw the unrecognized exception.
                    }
    
    
                    // [A.8] Throw an exception if transient SqlExceptions caused us
                    // to exceed our self-imposed maximum of retries.
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
                } // for cc
                return;
            } // method ConnectAndQuery
    
    
            void EstablishConnection()
            {
                try
                {
                    // [C.1]
                    using ( this.rsConnection = new E.ReliableSqlConnection(
                        this.scsBuilder.ToString(),
                        this.connectionRetryPolicy,
                        this.commandRetryPolicy )
                        )
                    {
                        this.progressLocation = M_progressCreatingConnection;
                        this.rsConnection.Open(); // [C.2] Open the newly constructed reliable connection.
    
                        // [C.3] Prepare delegate, then issue a query command.
                        this.commandRetryPolicy.ExecuteAction(this.IssueQueryCommand_action);
                    }
                }
                // TEST.PASSWORD_FIX.
                    // Step_1: In InitializeFields(), set password to a wrong value.
                    // Step_2: Uncomment the following 'catch' of sqlExc.
                //catch (C.SqlException sqlExc)  // For TESTING only.  Usually comment out this one CATCH.
                //{
                //    if (sqlExc.Number == 18456)  // Could mean wrong password, could not connect.
                //    {
                //        this.scsBuilder["Password"] = "MyCorrectPassword";
                //        Console.WriteLine("Testing.  Password now fixed in catch.");
                //    }
                //    throw sqlExc;
                //}
                catch (Exception exc)
                {
                    throw exc;  // [C.4] Let caller assess any exception, SqlException or any kind.
                }
                return;
            } // method EstablishConnection
    
    
            void IssueQueryCommand()
            {
                D.IDataReader dReader = null;
                D.IDbCommand dbCommand = null;
    
                X.StringBuilder sBuilder = new X.StringBuilder(256);
    
                try
                {
                    // [D.1] Use the connection to create a query command.
                    using (dbCommand = this.rsConnection.CreateCommand())
                    {
                        dbCommand.CommandText = this.sqlSelectCode;
    
                        // [D.2] Issue the query command through the connection.
                        this.progressLocation = M_progressIssuingCommand;
                        using (dReader = this.rsConnection.ExecuteCommand<D.IDataReader>(dbCommand))
                        {
                            Console.WriteLine();
                            // [D.3] Loop through all returned rows, writing the data to the console.
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
                    throw exc; // [D.4] Let caller assess any exception.
                }
                return;
            } // method IssueQueryCommand
    
    
            void InitializeFields()
            {
                // [B.1] Prepare the connection string to Azure SQL Database.
                this.scsBuilder = new C.SqlConnectionStringBuilder();
                // Change these values to your values.
                this.scsBuilder["Server"] = "tcp:myazuresqldbserver.database.windows.net,1433";
                this.scsBuilder["User ID"] = "MyLogin";  // @yourservername suffix sometimes.
                this.scsBuilder["Password"] = "MyCorrectPassword";  // TEST.PASSWORD_FIX.
                this.scsBuilder["Database"] = "MyDatabase";
                // Leave these values as they are.
                this.scsBuilder["Trusted_Connection"] = false;
                this.scsBuilder["Integrated Security"] = false;
                this.scsBuilder["Encrypt"] = true;
                this.scsBuilder["Connection Timeout"] = 30; // Default is 15 seconds, too brief over Internet!
    
                // [B.2] Prepare the Transact-SQL select statement.
                this.sqlSelectCode =
                    @"SELECT TOP 3 name, CAST(object_id as nvarchar(32)) as [object_id]
                      FROM sys.objects
                      WHERE type='IT'
                      ORDER BY name;";
    
                // [B.3] Begin steps to create a retry policy for connecting.
                this.exponentialBackoffStrategy = new E.ExponentialBackoff(
                    "exponentialBackoffStrategy",
                    2,                               // Maximum number of times to retry.
                    TimeSpan.FromMilliseconds(2000), // Minimum interval between retries.
                    TimeSpan.FromMilliseconds(8000), // Maximum interval between retries.
                    TimeSpan.FromMilliseconds(2000)  // Factor for random differences in interval between retries.
                    );
    
                this.forcePersistent_SqlCommandTransientErrorDetectionStrategy =
                    new ForcePersistent_SqlCommandTransientErrorDetectionStrategy();
    
                this.custom_SqlDatabaseTransientErrorDetectionStrategy =
                    new Custom_SqlDatabaseTransientErrorDetectionStrategy();// TEST.PASSWORD_FIX.
    
                this.connectionRetryPolicy = new E.RetryPolicy(
                    this.custom_SqlDatabaseTransientErrorDetectionStrategy,
                    this.exponentialBackoffStrategy);
                this.commandRetryPolicy = new E.RetryPolicy(
                    this.forcePersistent_SqlCommandTransientErrorDetectionStrategy,
                    this.exponentialBackoffStrategy);
    
                this.EstablishConnection_action = delegate() { this.EstablishConnection(); };
                this.IssueQueryCommand_action   = delegate() { this.IssueQueryCommand(); };
    
                return;
            } // method InitializeFields
        } // class Program
    }


&nbsp;


## `Custom_SqlDatabaseTransientErrorDetectionStrategy.cs` 文件


EntLib60 具有名为的类`ReliableSqlConnection`。 您可以控制如何连接实例确定异常是否为它分配实现的类是暂时性错误`ITransientErrorDetectionStrategy`。

类提供了 EntLib60 `SqlDatabaseTransientErrorDetectionStrategy`。 但在本文中，我们选择实现和使用我们自己的自定义类`Custom_SqlDatabaseTransientErrorDetectionStrategy`。 我们自定义的类具有白名单要比较的值`SqlException.Number`。


    using     System;
    using G = System.Collections.Generic;
    using C = System.Data.SqlClient;
    using E = Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling;
    
    namespace ConsoleApplication1
    {
        /// <summary>
        /// A custom alternative to class SqlDatabaeTransientErrorDetectionStrategy.
        /// </summary>
        public class Custom_SqlDatabaseTransientErrorDetectionStrategy
            : E.ITransientErrorDetectionStrategy
        {
            static private G.List<int> M_listTransientErrorNumbers;
    
    
            /// <summary>
            /// This method is required by ITransientErrorDetectionStrategy.
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
                    {4060, 10928, 10929, 40197, 40501, 40613};
    
                M_listTransientErrorNumbers = new G.List<int>(arrayOfTransientErrorNumbers);
            }
        } // class Custom_SqlDatabaseTransientErrorDetectionStrategy
    }


&nbsp;


## `ForcePersistent_SqlCommandTransientErrorDetectionStrategy.cs` 文件


该类实现的自定义`ITransientErrorDetectionStrategy`强制我们选择永远不会考虑让 EntLib60 考虑任何查询命令错误是只是暂时的。 相反，总体计划旨在评估`SqlException.Number`在其自己的自定义逻辑。


使用本部分中的类，我们进行设计的选择让我们计划放弃并重新创建新的连接，然后重试失败的查询的任何命令，瞬态或不。


    using     System;
    using E = Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling;
    
    
    namespace ConsoleApplication1
    {
        /// <summary>
        /// Is a substitute for class E.SqlCommandTransientErrorDetectionStrategy.
        /// Choosing to always discard the Sql Connection whenever the command fails,
        /// regardless of whether command failure was transient.
        /// Program try/catch and for-loop logic prefers to discard then re-construct
        /// a new Sql Connection.
        /// </summary>
        public class ForcePersistent_SqlCommandTransientErrorDetectionStrategy
            : E.ITransientErrorDetectionStrategy
        {
            public bool IsTransient(Exception exc)
            {
                return false;  // Force a failure, even if is transient.  No retry of sql command.
            }
        }
    }


&nbsp;


## 测试您的重试逻辑


很难测试 EntLib60 程序。 它很难导致真正暂时性错误，并控制其计时。 这段演示代码包含一个简单的方法可用来模拟瞬态错误。 


方法是使用我们的自定义检测器`Test2_TransientErrorDetectionStrategy`类代替`Custom_SqlTransientErrorDetectionStrategy`类。 通过这种方式，您可以强制`ReliableSqlConnection`EntLib60 来评估*所有*的`SqlException`为瞬态的匹配项。 这一轮是有自我解决问题首次重试之前的程序。


### 拼写错误的密码技巧


一种简单的方法是启动连接密码的拼写错误值。 这将导致当`SqlException`，与`sqlExc.Number==18456`，该程序必须修复该密码。 在重试计划就会成功。


若要在我们的演示程序中实现此测试，源代码中搜索的所有匹配项`TEST.PASSWORD_FIX`。 找到的所有匹配项之后`TEST.PASSWORD_FIX`和进行解释的更改在每个位置的注释中，将进行以下更改︰


- 实时代码中`InitializeFields`方法，将密码值更改为正确的值。

- 在`//`注释在代码中，`EstablishConnection`方法中，没有一个完整`catch`块...<br/>`//catch (SDSqlC.SqlException sqlExc)`<br/>它被注释掉。 取消注释的整个块。

- 在没有新批注`catch`块，将正确的密码值。

- 其中类`Custom_TransientErrorDetectionStrategy`是在代码中引用，替换具有引用`Test2_TransientErrorDetectionStrategy`。 您可以相同保留的变量名。


### `Test2_TransientErrorDetectionStrategy.cs` 文件


    using     System;
    using E = Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling;
    
    namespace ConsoleApplication1
    {
        /// <summary>
        /// For testing, is a substitute for class E.SqlDatabaseTransientErrorDetectionStrategy.
        /// Change the true/false assignment to returnValue for different tests.
        /// </summary>
        public class Test2_TransientErrorDetectionStrategy
            : E.ITransientErrorDetectionStrategy
        {
            public bool IsTransient(Exception exc)
            {
                bool returnValue = Test2_TransientErrorDetectionStrategy.IsTransientStatic(exc);
                Console.WriteLine("Inside Test2_TransientErrorDetectionStrategy .IsTransientStatic,"
                    + " returning {0}.", returnValue);
                return returnValue;
            }
            static public bool IsTransientStatic(Exception exc)
            {
                return true;  //false;
            }
        }
    }


&nbsp;


## 短`Program.cs`，而不 `try/catch`


本节中的代码短会编译和运行它通常会正常工作。 但是短代码不是在生产中运行。


这样做的唯一目的短`Program.cs`示例是为了便于在实际中的同一个 EntLib 调用，请参阅`Program.cs`。 删除所有的`try/catch`阻止的使 EntLib 调用更易于查看。


    using     System;  // C#
    using X = System.Text;
    using D = System.Data;
    using C = System.Data.SqlClient;
    using E = Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling;
    
    namespace ConsoleApplication1_Shorter_No_TryCatch
    {
        class Program
        {
            private E.ReliableSqlConnection rsConnection;
            private E.RetryPolicy connectionRetryPolicy;
            private E.RetryPolicy commandRetryPolicy;
            private E.ExponentialBackoff exponentialBackoffStrategy;
            private C.SqlConnectionStringBuilder scsBuilder;
            private Action EstablishConnection_action;
            private Action IssueQueryCommand_action;
            private string sqlSelectCode;
            private Custom_SqlDatabaseTransientErrorDetectionStrategy
                custom_SqlDatabaseTransientErrorDetectionStrategy;
            private ForcePersistent_SqlCommandTransientErrorDetectionStrategy
                forcePersistent_SqlCommandTransientErrorDetectionStrategy;
    
            static void Main(string[] args)
            {
                new Program().ConnectAndQuery();
            }
    
            void ConnectAndQuery()
            {
                this.InitializeFields();
                this.connectionRetryPolicy.ExecuteAction(this.EstablishConnection_action);
            } // method ConnectAndQuery
    
            void EstablishConnection()
            {
                using ( this.rsConnection = new E.ReliableSqlConnection(
                    this.scsBuilder.ToString(),
                    this.connectionRetryPolicy,
                    this.commandRetryPolicy )
                        )
                {
                    this.rsConnection.Open();
                    this.commandRetryPolicy.ExecuteAction(this.IssueQueryCommand_action);
                }
            } // method EstablishConnection
    
    
            void IssueQueryCommand()
            {
                D.IDataReader dReader = null;
                D.IDbCommand dbCommand = null;
                X.StringBuilder sBuilder = new X.StringBuilder(256);
    
                using (dbCommand = this.rsConnection.CreateCommand())
                {
                    dbCommand.CommandText = this.sqlSelectCode;
                    using (dReader = this.rsConnection.ExecuteCommand<D.IDataReader>(dbCommand))
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
    
            void InitializeFields()
            {
                this.scsBuilder = new C.SqlConnectionStringBuilder();
                // Change these values to your values.
                this.scsBuilder["Server"] = "tcp:myazuresqldbserver.database.windows.net,1433";
                this.scsBuilder["User ID"] = "MyLogin";
                this.scsBuilder["Password"] = "MyPassword";  // TEST.PASSWORD_FIX.
                this.scsBuilder["Database"] = "MyDatabase";
                // Leave these values as they are.
                this.scsBuilder["Trusted_Connection"] = false;
                this.scsBuilder["Integrated Security"] = false;
                this.scsBuilder["Encrypt"] = true;
                this.scsBuilder["Connection Timeout"] = 30;
    
                this.sqlSelectCode =
                    @"SELECT TOP 3 name, CAST(object_id as nvarchar(32)) as [object_id]
                      FROM sys.objects
                      WHERE type='IT'
                      ORDER BY name;";
    
                this.exponentialBackoffStrategy = new E.ExponentialBackoff(
                    "exponentialBackoffStrategy",
                    2,
                    TimeSpan.FromMilliseconds(2000),
                    TimeSpan.FromMilliseconds(8000),
                    TimeSpan.FromMilliseconds(2000)
                    );
    
                this.forcePersistent_SqlCommandTransientErrorDetectionStrategy =
                    new ForcePersistent_SqlCommandTransientErrorDetectionStrategy();
                this.custom_SqlDatabaseTransientErrorDetectionStrategy =
                    new Custom_SqlDatabaseTransientErrorDetectionStrategy();
    
                this.connectionRetryPolicy = new E.RetryPolicy(
                    this.custom_SqlDatabaseTransientErrorDetectionStrategy,
                    this.exponentialBackoffStrategy);
                this.commandRetryPolicy = new E.RetryPolicy(
                    this.forcePersistent_SqlCommandTransientErrorDetectionStrategy,
                    this.exponentialBackoffStrategy);
    
                this.EstablishConnection_action = delegate() { this.EstablishConnection(); };
                this.IssueQueryCommand_action   = delegate() { this.IssueQueryCommand(); };
            } // method InitializeFields
        } // class Program
    }


## 相关的链接


- [Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling Namespace](http://msdn.microsoft.com/library/microsoft.practices.enterpriselibrary.transientfaulthandling.aspx)

- [企业库 6 类库](http://msdn.microsoft.com/library/dn170426.aspx)

- [代码示例︰ 用于连接到 SQL 数据库与 ADO.NET 试中 C# 的逻辑](sql-database-develop-csharp-retry-windows.md)

- [对 SQL 数据库的客户端快速启动代码示例](sql-database-develop-quick-start-client-code-samples.md)

