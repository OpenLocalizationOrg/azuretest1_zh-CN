---
ms.openlocfilehash: bcecb1b7178dd4d4a6af414a02b653fb4c6a206b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
这些步骤创建到该应用程序发送推式通知的新自定义[ApiController](http://go.microsoft.com/fwlink/p/?LinkId=512673) 。 在[TableController](http://msdn.microsoft.com/library/azure/dn643359.aspx)中或后端服务的其他任何地方，您可以实现此相同的代码。 

1. Visual Studio 解决方案资源管理器，右键单击控制器文件夹移动服务项目，展开**添加**，然后单击**启用了基架的新项**。

    这将显示对话框中添加的构架。

2. 展开**移动服务 Azure** **Azure 移动服务自定义控制器**，然后单击**添加**提供的**控制器名称** `NotifyAllUsersController`，然后单击**添加**。

    ![Web API 添加构架对话框](./media/mobile-services-dotnet-backend-update-server-push-vs2013/add-custom-api-controller.png)

    这将创建一个名为**NotifyAllUsersController**的新空的控制器类。 

3. 在新的 NotifyAllUsersController.cs 项目文件中添加下面的**using**语句︰

        using Newtonsoft.Json.Linq;
        using System.Threading.Tasks;

4. 添加下面的代码实现的控制器上的 POST 方法︰

        public async Task<bool> Post(JObject data)
        {
            try
            {
                // Define the XML paylod for a WNS native toast notification 
                // that contains the value supplied in the POST request.
                string wnsToast = 
                    string.Format("<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<toast><visual><binding template=\"ToastText01\">" + 
                    "<text id=\"1\">{0}</text></binding></visual></toast>", 
                    data.GetValue("toast").Value<string>());

                // Define the WNS native toast with the payload string.
                WindowsPushMessage message = new WindowsPushMessage();
                message.XmlPayload = wnsToast;

                // Send the toast notification.
                await Services.Push.SendAsync(message);
                return true;
            }
            catch (Exception e)
            {
                Services.Log.Error(e.ToString());
            }
            return false;
        }

    >[AZURE.NOTE]可以通过任何客户端都是不安全的应用程序键调用此 POST 方法。 若要保护该终结点，请将该属性应用`[AuthorizeLevel(AuthorizationLevel.User)]`对方法或类时要求身份验证。 
