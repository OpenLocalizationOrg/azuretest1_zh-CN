---
ms.openlocfilehash: b751df0ca0226f61298cbb045feffb9f806a9506
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 在解决方案资源管理器在 Visual Studio 中，展开 App_Start 文件夹，并打开 WebApiConfig.cs 项目文件。

2. **ConfigOptions**定义之后将以下代码行添加到注册方法︰

        options.PushAuthorization = 
            Microsoft.WindowsAzure.Mobile.Service.Security.AuthorizationLevel.User;
 
    这将强制用户身份验证前推式通知的注册。 

2. 用鼠标右键单击该项目，单击**添加**，然后单击**类...**。

3. 命名新的空类`PushRegistrationHandler`然后单击**添加**。

4. 在代码页顶部，添加下面的**using**语句︰

        using System.Threading.Tasks; 
        using System.Web.Http; 
        using System.Web.Http.Controllers; 
        using Microsoft.WindowsAzure.Mobile.Service; 
        using Microsoft.WindowsAzure.Mobile.Service.Notifications; 
        using Microsoft.WindowsAzure.Mobile.Service.Security; 

5. 现有的**PushRegistrationHandler**类替换为以下代码︰
 
        public class PushRegistrationHandler : INotificationHandler
        {
            public Task Register(ApiServices services, HttpRequestContext context,
            NotificationRegistration registration)
            {
                try
                {
                    // Perform a check here for user ID tags, which are not allowed.
                    if(!ValidateTags(registration))
                    {
                        throw new InvalidOperationException(
                            "You cannot supply a tag that is a user ID.");                    
                    }
    
                    // Get the logged-in user.
                    var currentUser = context.Principal as ServiceUser;
    
                    // Add a new tag that is the user ID.
                    registration.Tags.Add(currentUser.Id);
    
                    services.Log.Info("Registered tag for userId: " + currentUser.Id);
                }
                catch(Exception ex)
                {
                    services.Log.Error(ex.ToString());
                }
                    return Task.FromResult(true);
            }
    
            private bool ValidateTags(NotificationRegistration registration)
            {
                // Create a regex to search for disallowed tags.
                System.Text.RegularExpressions.Regex searchTerm =
                new System.Text.RegularExpressions.Regex(@"facebook:|google:|twitter:|microsoftaccount:",
                    System.Text.RegularExpressions.RegexOptions.IgnoreCase);
    
                foreach (string tag in registration.Tags)
                {
                    if (searchTerm.IsMatch(tag))
                    {
                        return false;
                    }
                }
                return true;
            }
        
            public Task Unregister(ApiServices services, HttpRequestContext context, 
                string deviceId)
            {
                // This is where you can hook into registration deletion.
                return Task.FromResult(true);
            }
        }

    在注册过程中调用**注册**方法。 这允许您将标记添加到注册登录的用户的 id。 提供的标记进行验证来防止用户在注册为另一个用户 id。 当向该用户发送通知时，收到这和注册用户的任何其他设备上。 

6. 展开控制器文件夹打开 TodoItemController.cs 项目文件、 **PostTodoItem**方法，请替换为以下代码调用**SendAsync**的代码行︰

        // Get the logged-in user.
        var currentUser = this.User as ServiceUser;
        
        // Use a tag to only send the notification to the logged-in user.
        var result = await Services.Push.SendAsync(message, currentUser.Id);

7. 重新发布的移动服务项目。

现在，服务使用的用户 ID 标记发送推式通知 （带有插入的项的文本） 向已登录的用户创建的所有注册事件。
 