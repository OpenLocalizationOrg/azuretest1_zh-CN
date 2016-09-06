---
ms.openlocfilehash: c15f8b138247892a71c18f699f4f89c241a12b81
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 在 Visual Studio 解决方案资源管理器中，展开手机信息服务项目中的**控制器**文件夹。 打开 TodoItemController.cs 并更新`PostTodoItem`方法定义为以下代码︰  

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);

            // Create a WNS native toast.
            WindowsPushMessage message = new WindowsPushMessage();

            // Define the XML paylod for a WNS native toast notification 
            // that contains the text of the inserted item.
            message.XmlPayload = @"<?xml version=""1.0"" encoding=""utf-8""?>" +
                                 @"<toast><visual><binding template=""ToastText01"">" +
                                 @"<text id=""1"">" + item.Text + @"</text>" +
                                 @"</binding></visual></toast>";
            try
            {
                var result = await Services.Push.SendAsync(message);
                Services.Log.Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                Services.Log.Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

    此代码将插入一个 todo 项后发送推式通知 （带有插入的项的文本）。 如果出现错误，代码将添加哪些移动服务管理门户中的**日志**选项卡上可以查看错误日志项。

    >[AZURE.NOTE] 可以使用模板通知多个平台上的客户端发送一个推式通知。 有关详细信息，请参阅[支持多个设备的平台，从单一的移动服务](../articles/mobile-services-how-to-use-multiple-clients-single-service.md#push)。

2. 重新发布到 Azure 的移动服务项目。


