---
ms.openlocfilehash: 92a57f14fb909036287461c358e25e12136be9da
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


1. 在 Visual Studio 中，用鼠标右键单击控制器文件夹，展开**添加**，然后单击**启用了基架的新项**。 这将显示对话框中添加的构架。

2. 展开**Azure 移动服务**，单击**Azure 移动服务自定义控制器**，单击**添加**，提供的**控制器名称** `CompleteAllController`，然后单击**添加**。

    ![Web API 添加构架对话框](./media/mobile-services-dotnet-backend-create-custom-api/add-custom-api-controller.png)

    这将创建一个名为**CompleteAllController**的新空的控制器类。

    >[AZURE.NOTE]如果对话中没有移动的服务特定的支架，而是创建一个新的**Web API 控制器-空**。 在此新的控制器类添加一个公共的**服务**属性，它返回**ApiServices**类型。 此属性用于访问特定于服务器的设置从您的控制器。

3. 在**CompleteAllController.cs**中，添加下面的**using**语句。     更换`todolistService`与您的移动服务项目的命名空间，它应该被移动服务名称后面附加了`Service`。

        using System.Threading.Tasks;
        using todolistService.Models;

4. 在**CompleteAllController.cs**中，添加下面的类来包装发送到客户端的响应。

        // We use this class to keep parity with other Mobile Services
        // that use the JavaScript backend. This way the same client
        // code can call either type of Mobile Service backend.
        public class MarkAllResult
        {
            public int count;
        }

5. 将以下代码添加到新的控制器。 更换`todolistContext`数据模型 DbContext 的名称，这应该是移动服务名称后面附加了`Context`。 同样，您的移动服务的名称替换更新语句中的架构名。 此代码使用[数据库类](http://msdn.microsoft.com/library/system.data.entity.database.aspx)来访问**TodoItems**表直接设置在所有项目上已完成的标志。 此方法支持 POST 请求，并且已更改的行数返回给客户端作为一个整数值。


        // POST api/completeall
        public async Task<MarkAllResult> Post()
        {
            using (todolistContext context = new todolistContext())
            {
                // Get the database from the context.
                var database = context.Database;

                // Create a SQL statement that sets all uncompleted items
                // to complete and execute the statement asynchronously.
                var sql = @"UPDATE todolist.TodoItems SET Complete = 1 " +
                            @"WHERE Complete = 0; SELECT @@ROWCOUNT as count";

                var result = new MarkAllResult();
                result.count = await database.ExecuteSqlCommandAsync(sql);

                // Log the result.
                Services.Log.Info(string.Format("{0} items set to 'complete'.",
                    result.count.ToString()));

                return result;
            }
        }

    > [AZURE.NOTE] 使用默认的权限，与应用程序的键的任何人都可以调用自定义的 API。 但是，应用程序键因为它可能不会分发或安全地存储不会认为安全的凭据。 考虑到只有已通过身份验证用户对于附加的安全性限制访问。
