---
ms.openlocfilehash: e3c0b91da8a281727e5d36c5bd96d31b56da59cd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

它始终是一个好的习惯，来验证用户提交的数据的长度。 在本节中，您将添加代码来验证字符串数据的长度的移动服务发送到移动服务和拒绝都太长，在这种情况下超过 10 个字符的字符串。

1. 使用**以管理员身份运行**选项启动 Visual Studio 并打开包含与合作[入门]或[数据入门](../articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md)教程中的移动服务项目的解决方案。

2. 在解决方案资源管理器窗口中展开 todo 列表服务项目，然后展开**控制器**。 打开 TodoItemController.cs 文件，这是移动服务项目的一部分。  

    ![](./media/mobile-services-dotnet-backend-add-validation/mobile-services-open-todoitemcontroller.png)

3. 更换`PostTodoItem`具有下面的方法将不超过 10 个字符的文本字符串进行验证的方法。 对于有多于 10 个字符长度的文本的项，该方法返回的 HTTP 状态代码作为内容的描述性消息 400 错误的请求。


        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            if (item.Text.Length > 10)
            {
                return BadRequest("The Item's Text length must not be greater than 10.");
            }
            else
            {
                TodoItem current = await InsertAsync(item);
                return CreatedAtRoute("Tables", new { id = current.Id }, current);
            } 
        }



4. 右键单击该服务项目，然后单击**生成**以生成移动服务项目。 验证不发生任何错误。

    ![](./media/mobile-services-dotnet-backend-add-validation/mobile-services-build-dotnet-service.png)

5. 右键单击该服务项目，然后单击**发布**。 将移动服务发布到您的 Microsoft Azure 帐户使用您以前使用[入门]或[数据入门](../articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md)教程中的发布设置。
 
     >[AZURE.NOTE] 也可以在 IIS Express 本地承载的服务进行测试。 详细信息请参阅[有关数据入门](../articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md)教程。

    ![](./media/mobile-services-dotnet-backend-add-validation/mobile-services-publish-dotnet-service.png)





<!-- URLs. -->
[入门教程]: ../articles/mobile-services/mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
