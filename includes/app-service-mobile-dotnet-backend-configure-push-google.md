---
ms.openlocfilehash: 5fb6897cee6bfe6f672c6ac1052a5a83674695fc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 在 Visual Studio**解决方案资源管理器**中展开移动后端项目中的**控制器**文件夹。 打开**TodoItemController.cs**。 在文件的顶部，添加以下`using`语句︰

        using System.Collections.Generic;        
        using Microsoft.Azure.Mobile.Server.Notifications;


2. 更换`PostTodoItem`方法与下列代码︰  
        
        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);

            Dictionary<string, string> data = new Dictionary<string, string>()
            {
                { "message", item.Text}
            };

            HttpConfiguration config = this.Configuration;

            GooglePushMessage message = new GooglePushMessage(data, System.TimeSpan.FromHours(1));

            try
            {
                var client = new PushClient(config);
                var result = await client.SendAsync(message);

                ServiceSettingsDictionary settings = config.GetServiceSettingsProvider().GetServiceSettings();
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                config.Services.GetTraceWriter().Error(ex.Message, null, "Push.SendAsync Error");
            }

            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

