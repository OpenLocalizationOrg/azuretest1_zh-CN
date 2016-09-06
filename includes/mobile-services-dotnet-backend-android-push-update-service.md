---
ms.openlocfilehash: 34a48fa3b5b4d0c370fab598b398327e98109f84
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 在 Visual Studio 解决方案资源管理器中，展开手机信息服务项目中的**控制器**文件夹。 打开 TodoItemController.cs。 在文件的顶部，添加以下`using`语句︰

        using System;
        using System.Collections.Generic;

2. 更新`PostTodoItem`方法定义为以下代码︰  

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);

            Dictionary<string, string> data = new Dictionary<string, string>()
            {
                { "message", item.Text}
            };
            GooglePushMessage message = new GooglePushMessage(data, TimeSpan.FromHours(1));

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

3. 重新发布到 Azure 的移动服务项目。