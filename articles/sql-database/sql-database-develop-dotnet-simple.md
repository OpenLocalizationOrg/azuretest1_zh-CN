---
ms.openlocfilehash: 667e3b5cfbc9cd599d86cf9ac82e345a2f9f7a96
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 SQL 数据库从.NET (C#)" 
    description="使用示例代码在此快速开始构建现代应用程序与 C# 并在云中使用 SQL Azure 数据库强大关系数据库作后盾。"
    services="sql-database" 
    documentationCenter="" 
    authors="tobbox" 
    manager="jeffreyg" 
    editor=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="tobiast"/>


# 使用 SQL 数据库从.NET (C#) 


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


## 要求

### .NET Framework

.NET Framework 是预安装 Windows。 对于 Linux 和 Mac OS X 可以下载.NET Framework，从[单声道项目](http://www.mono-project.com/)。

### SQL 数据库

请参阅[入门指南](sql-database-get-started.md)以了解如何创建示例数据库并获取您的连接字符串。  

## 连接到 SQL 数据库

[System.Data.SqlClient.SqlConnection 类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx)用于连接到 SQL 数据库。  
    
```
using System.Data.SqlClient;

class Sample
{
  static void Main()
  {
      using(var conn = new SqlConnection("Server=tcp:yourserver.database.windows.net,1433;Database=yourdatabase;User ID=yourlogin@yourserver;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"))
      {
          conn.Open();  
      }
  }
}   
```

## 执行查询并检索结果集 

[System.Data.SqlClient.SqlCommand](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx)和[SqlDataReader](https://msdn.microsoft.com/library/system.data.sqlclient.sqldatareader.aspx)类可以用于检索结果集对 SQL 数据库的查询。 请注意，System.Data.SqlClient 还支持离线[System.Data.DataSet](https://msdn.microsoft.com/library/system.data.dataset.aspx)中检索数据。   
    
```
using System;
using System.Data.SqlClient;

class Sample
{
    static void Main()
    {
      using(var conn = new SqlConnection("Server=tcp:yourserver.database.windows.net,1433;Database=yourdatabase;User ID=yourlogin@yourserver;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"))
        {
            var cmd = conn.CreateCommand();
            cmd.CommandText = @"
                    SELECT 
                        c.CustomerID
                        ,c.CompanyName
                        ,COUNT(soh.SalesOrderID) AS OrderCount
                    FROM SalesLT.Customer AS c
                    LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID
                    GROUP BY c.CustomerID, c.CompanyName
                    ORDER BY OrderCount DESC;";

            conn.Open();    
        
            using(var reader = cmd.ExecuteReader())
            {
                while(reader.Read())
                {
                    Console.WriteLine("ID: {0} Name: {1} Order Count: {2}", reader.GetInt32(0), reader.GetString(1), reader.GetInt32(2));
                }
            }                   
        }
    }
}

```

## 插入行、 传递的参数，以及检索生成的主键值 

在 SQL 数据库中[标识](https://msdn.microsoft.com/library/ms186775.aspx)属性和[SEQUENECE](https://msdn.microsoft.com/library/ff878058.aspx)的对象可用于自动生成[主键](https://msdn.microsoft.com/library/ms179610.aspx)的值。 在本示例中将了解如何执行的[insert 语句](https://msdn.microsoft.com/library/ms174335.aspx)，则安全地传递参数的防止[SQL 注入](https://msdn.microsoft.com/magazine/cc163917.aspx)，并检索自动生成的主键值。  

在[System.Data.SqlClient.SqlCommand](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx)类中的[ExecuteScalar](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executescalar.aspx)方法可以用于执行语句并检索第一列，该语句返回的行。 [OUTPUT](https://msdn.microsoft.com/library/ms177564.aspx)子句的 INSERT 语句可用于返回到调用应用程序与结果集的插入的值。 请注意，输出也被支持语句的[更新](https://msdn.microsoft.com/library/ms177523.aspx)、[删除](https://msdn.microsoft.com/library/ms189835.aspx)和[合并](https://msdn.microsoft.com/library/bb510625.aspx)。 如果多个行插入您应该使用[ExecuteReader](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executereader.aspx)方法来检索所有行的插入的值。
    
```
class Sample
{
    static void Main()
    {
        using(var conn = new SqlConnection("Server=tcp:yourserver.database.windows.net,1433;Database=yourdatabase;User ID=yourlogin@yourserver;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"))
        {
            var cmd = conn.CreateCommand();
            cmd.CommandText = @"
                INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) 
                OUTPUT INSERTED.ProductID
                VALUES (@Name, @Number, @Cost, @Price, CURRENT_TIMESTAMP)";

            cmd.Parameters.AddWithValue("@Name", "SQL Server Express");
            cmd.Parameters.AddWithValue("@Number", "SQLEXPRESS1");
            cmd.Parameters.AddWithValue("@Cost", 0);
            cmd.Parameters.AddWithValue("@Price", 0);

            conn.Open();

            int insertedProductId = (int)cmd.ExecuteScalar();

            Console.WriteLine("Product ID {0} inserted.", insertedProductId);
        }
    }
}
```

 
