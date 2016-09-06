---
ms.openlocfilehash: 5642e5657b01a49fddc8cf2e0bbc19dc84a004c5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 CSharp SQL Azure 数据库上创建程序集"
    description="提供 C# 源代码将创建程序集后第一个到包含长的十六进制数的字符串编码 DLL 文件发送到 SQL Azure 数据库。" 
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
    ms.date="07/14/2015" 
    ms.author="genemi"/>


# 在 CSharp SQL Azure 数据库上创建程序集


<!--
GeneMi , Latest edit = 2015-March-25  Wednesday  10:22am
Converting plain text "CREATE ASSEMBLY" into a link to the MSDN topic, ms189524.aspx. And ms186755.aspx for "CREATE FUNCTION".
-->


本主题提供了可用于[创建程序集](http://msdn.microsoft.com/library/ms189524.aspx)语句发出的 SQL Azure 数据库的 C# 代码示例。 对于 SQL 数据库，FROM 子句不能接受简单格式的上本地承载数据库的计算机的路径。 另一种方法是首先进行编码转换包含一个十六进制数字为长字符串的程序集 DLL 的二进制位。 然后为 FROM 子句中的值为字符串。


### 先决条件


要了解本主题，您必须具有已部分知道以下︰


- [CLR 表值函数](http://msdn.microsoft.com/library/ms131103.aspx)<br/>解释如何创建程序集的事务处理 SQL 语句工作与其它语句的内部部署 Microsoft SQL Server。


## 答︰ 整体技术


1. 如果需要清除上次运行，删除函数，删除程序集。
2. 请记住您根据您自己的代码编译 Microsoft.NET Framework 程序集 DLL 文件的位置。 您提供的下一步的位置。 
3. 给出 C# 源代码 EXE 运行本主题中。 告诉该 EXE DLL 文件的位置。
 - 为长字符串，其中包含一个十六进制数字编码二进制 DLL。
 - 问题创建程序集语句 FROM 子句中指定的十六进制字符串。
4. [创建函数](http://msdn.microsoft.com/library/ms186755.aspx)来引用程序集中的方法。
5. T-SQL SELECT 语句来调用并测试函数。


前面的列表使没有提到...<br/>
**执行 sp_configure clr 启用'，1;**<br/>
.这不需要 Azure SQL 数据库，即使 Microsoft SQL Server 需要的.because。


如果需要重新运行的节目，将为以下 T SQL 代码以删除函数和程序集︰


    DROP FUNCTION fnCompareStringsCaseSensitive;
    DROP ASSEMBLY CreateAssemblyFunctions3;


## B。 简单的程序集引用 T SQL 函数的 DLL


本节中的常用 C# 代码示例可编译到程序集 DLL 文件。


此代码示例包含**CompareCaseSensitiveNet**以后在 T SQL 创建函数语句中引用的方法。 请注意，方法用名为**SqlFunction**的.NET 属性修饰。 用此属性修饰的方法可以作为函数调用由您 T SQL。


    using           System;   // C#
    using SDSqTyp = System.Data.SqlTypes;
    using MSqServ = Microsoft.SqlServer.Server;
    
    namespace CreateAssemblyFunctions3
    {
        public class SqlFunctionMethodsForSprocs
        {
            /// <summary> This method is referenced
            /// by a T-SQL CREATE FUNCTION statement. </summary>
            [MSqServ.SqlFunction(IsDeterministic = true, IsPrecise = true)]
            static public SDSqTyp.SqlInt32 CompareCaseSensitiveNet(string strA, string strB)
            {
                return String.Compare(strA, strB, false);
            }
        }
    }


## C。 C & #x 23;EXE，它会发出创建程序集的代码示例


当您运行该 C# 示例中生成的 EXE 时，按照下列顺序发生︰


1. EXE 命令行运行调用的**Main**方法。
2. 主调用**ObtainHexStringOfAssembly**方法。
 - 该方法将输出存储为十六进制数字的程序集以。
3. 主调用**SubmitCreateAssemblyToAzureSqlDb**方法。
 - 主要是以。
 - 输出被发送到 SQL Azure 数据库的创建程序集调用。


            using             System;   // C#
            using SDat      = System.Data;
            using SDSClient = System.Data.SqlClient;
            using SGlo      = System.Globalization;
            using SIo       = System.IO;
            using STex      = System.Text;
            
            namespace CreateAssemblyFromHexString6
            {
                /// <summary>
                /// Run the Main method on your local computer console, so it can issue a
                /// a CREATE ASSEMBLY statement to your Azure SQL Database server:
                /// </summary>
                class Program
                {
                    /// <summary>
                    /// Calls the methods in the proper sequence.
                    /// </summary>
                    /// <param name="args">
                    /// Parameters for the cmd.exe command line
                    ///    run of CreateAssemblyFromHexString6.exe:
                    /// args[0] = FullDirPathFileNameOfAssembly.
                    /// args[1] = AssemblyName.
                    ///    For the CREATE ASSEMBLY assemblyName statement.
                    /// args[2] = Azure SQL Database - ServerName, including a suffix like .database.windows.net .
                    /// args[3] = Azure SQL Database - DatabaseName.
                    /// args[4] = Azure SQL Database - LoginName.
                    /// args[5] = Azure SQL Database - Password for login.
                    ///    (Better if from .config file.)
                    /// </param>
                    static int Main(string[] args)
                    {
                        int returnCode = 1; // Only 0 (zero) means Good Success.
                        string
                            fullPathNameAssemblyFile,
                            assemblyName,
                            AzureSqlDbServerName,
                            AzureSqlDbDatabaseName,
                            AzureSqlDbLoginName,
                            AzureSqlDbPassword;
            
                        try
                        {
                            fullPathNameAssemblyFile = args[0];
                            assemblyName             = args[1];
                            AzureSqlDbServerName     = args[2];
                            AzureSqlDbDatabaseName   = args[3];
                            AzureSqlDbLoginName      = args[4];
                            AzureSqlDbPassword       = args[5];
            
                            string hexStringOfAssembly = Program
                                .ObtainHexStringOfAssembly(fullPathNameAssemblyFile);
            
                            Program.SubmitCreateAssemblyToAzureSqlDb(
                                hexStringOfAssembly,
                                assemblyName,
                                AzureSqlDbServerName,
                                AzureSqlDbDatabaseName,
                                AzureSqlDbLoginName,
                                AzureSqlDbPassword);
            
                            returnCode = 0;
                        }
                        catch (Exception ex)
                        {
                            Console.WriteLine("Bad, Failure.");
                            throw ex;
                        }
                        Console.WriteLine("Good, Success.");
                        return returnCode;
                    }
            
                    /// <summary> Encodes the binary bits of the assembly DLL into a 
                    /// string containing a hexadecimal number. </summary>
                    /// <param name="fullPathToAssembly"
                    /// >Full directory path plus the file name, to the .DLL file.</param>
                    /// <returns>A string containing a hexadecimal number that encodes
                    /// the binary bits of the .DLL file.</returns>
                    static private string ObtainHexStringOfAssembly
                        (string fullPathToAssembly)
                    {
                        STex.StringBuilder sbuilder = new STex.StringBuilder("0x");
                        using (SIo.FileStream fileStream = new SIo.FileStream(
                            fullPathToAssembly,
                            SIo.FileMode.Open, SIo.FileAccess.Read, SIo.FileShare.Read)
                            )
                        {
                            int byteAsInt;
                            while (true)
                            {
                                byteAsInt = fileStream.ReadByte();
                                if (-1 >= byteAsInt) { break; }
                                sbuilder.Append(byteAsInt.ToString
                                    ("X2", SGlo.CultureInfo.InvariantCulture));
                            }
                        }
                        return sbuilder.ToString();
                    }
            
                    /// <summary>
                    /// Sends a Transact-SQL CREATE ASSEMBLY FROM hexString.
                    /// </summary>
                    static private void SubmitCreateAssemblyToAzureSqlDb(
                        string hexStringOfAssembly,
                        string assemblyName,
                        string AzureSqlDbServerName,
                        string AzureSqlDbDatabaseName,
                        string AzureSqlDbLoginName,
                        string AzureSqlDbPassword)
                    {
                        string sqlCreateAssembly = "CREATE ASSEMBLY [" + assemblyName
                            + "] FROM " + hexStringOfAssembly + ";";
                        STex.StringBuilder sbuilderConnection = new STex.StringBuilder();
            
                        sbuilderConnection.Append("Server=tcp:");
                        sbuilderConnection.Append(AzureSqlDbServerName);
                        sbuilderConnection.Append(",1433;");
            
                        sbuilderConnection.Append("Database=");
                        sbuilderConnection.Append(AzureSqlDbDatabaseName);
                        sbuilderConnection.Append(";");
            
                        sbuilderConnection.Append("Trusted_Connection=False;");
                        sbuilderConnection.Append("Connection Timeout=30;");
            
                        sbuilderConnection.Append("User ID=");
                        sbuilderConnection.Append(AzureSqlDbLoginName);
                        sbuilderConnection.Append(";");
            
                        sbuilderConnection.Append("Password=");
                        sbuilderConnection.Append(AzureSqlDbPassword);
                        sbuilderConnection.Append(";");
            
                        using (SDSClient.SqlConnection sqlConnection =
                            new SDSClient.SqlConnection())
                        {
                            SDSClient.SqlCommand sqlCommand;
                            sqlConnection.ConnectionString = sbuilderConnection.ToString();
                            sqlCommand = sqlConnection.CreateCommand();
                            sqlCommand.CommandType = SDat.CommandType.Text;
                            sqlCommand.CommandText = sqlCreateAssembly;
                            sqlConnection.Open();
                            sqlCommand.ExecuteNonQuery();
                        }
                        return;
                    }
                }
            }


### C.1 编译引用和版本


当我们编译和测试代码样例以 EXE 工具时，我们使用以下︰


- Visual Studio 2013，更新 4
 - 我们的项目模板类型的简单控制台应用程序。
- .NET 4.5 framework


我们的 Visual Studio 项目引用了下列程序集的编译︰


- Microsoft.CSharp
- System
- System.Core
- 
                System.Data
- System.Data.DataSetExtensions
- System.Xml
- System.Xml.Linq


### C.2 命令行运行该 exe 文件


下面的代码块显示您可以输入要从控制台运行 EXE 命令行的示例。 为更好地显示人为这里包装中命令行参数。


    CreateAssemblyFromHexString6.exe
        C:\my\bin\debug\CreateAssemblyFunctions3.dll
        CreateAssemblyFunctions3
        myazuresqldbsvr2.database.windows.net
        myazuresqldbdab4
        myazurelogin
        Mypassword123


为解释简化本示例传递密码作为命令行参数。 更好的设计是从配置文件获取密码的 C# 代码。


## D. 运行创建函数语句


程序集存储在 SQL Azure 数据库服务器后，您必须运行事务处理 SQL 创建函数语句引用的程序集中的方法。


下面的事务处理 SQL 代码块包含几个不重要的 SELECT 语句，以显示数据库系统，为您的程序集和函数有记录证明。 最后是选择调用函数。


    SELECT a11.*, am2.*
        FROM           sys.assemblies       AS a11
            INNER JOIN sys.assembly_modules AS am2 ON am2.assembly_id = a11.assembly_id
        WHERE a11.name = 'CreateAssemblyFunctions3';
    GO
    
    CREATE FUNCTION fnCompareStringsCaseSensitive
        (@strA nvarchar(128), @strB nvarchar(128))
        returns int
        AS EXTERNAL NAME
            CreateAssemblyFunctions3
                .[CreateAssemblyFunctions3.SqlFunctionMethodsForSprocs]
                    .CompareCaseSensitiveNet;
    GO
    SELECT * FROM sys.objects WHERE name = 'fnCompareStringsCaseSensitive';
    
     -- Use the new function.
    SELECT dbo.fnCompareStringsCaseSensitive('BLUE', 'blue') as returnedValue;
    GO


在前面的事务处理 SQL 代码块以调用新函数的 SELECT 语句结束。 无法放置到存储过程的 SELECT 语句。


<!-- EndOfFile -->

 
