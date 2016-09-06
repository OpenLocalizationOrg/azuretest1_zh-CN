---
ms.openlocfilehash: 4c470042adec5b57cfaaef908c4e0ed2f1b59fa0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## WebAPI 项目

1. 在 Visual Studio 中，打开您在**通知用户**教程中创建的**AppBackend**项目。
2. 在 Notifications.cs 中，用下面的代码替换整个**通知**类。 请务必将占位符替换为连接字符串 （具有完全访问权限） 通知网络集线器，和该中心名。 您可以从[Azure 管理门户网站](http://manage.windowsazure.com)获取这些值。 本模块现在表示将发送的不同安全通知。 在完整实现，通知将存储在数据库中;为简单起见，在这种情况下我们将它们存储在内存中。

        public class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }
    
    
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
            
            private List<Notification> notifications = new List<Notification>();
    
            public NotificationHubClient Hub { get; set; }
    
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string payload)
            {
                var notification = new Notification() {
                Id = notifications.Count,
                Payload = payload,
                Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Notification ReadNotification(int id)
            {
                return notifications.ElementAt(id);
            }
        }

20. NotificationsController.cs，下面的代码替换的**NotificationsController**类定义中的代码。 此组件实现设备可靠，检索该通知的一种方法，还提供了一种方法 （出于本教程的目的） 来触发安全推到您的设备。 请注意，当将通知发送到通知中心，我们只发送通知 （和不实际的邮件） 的 ID 与原始通知︰

        public NotificationsController()
        {
            Notifications.Instance.CreateNotification("This is a secure notification!");
        }

        // GET api/notifications/id
        public Notification Get(int id)
        {
            return Notifications.Instance.ReadNotification(id);
        }

        public async Task<HttpResponseMessage> Post()
        {
            var secureNotificationInTheBackend = Notifications.Instance.CreateNotification("Secure confirmation.");
            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // windows
            var rawNotificationToBeSent = new Microsoft.ServiceBus.Notifications.WindowsNotification(secureNotificationInTheBackend.Id.ToString(),
                            new Dictionary<string, string> {
                                {"X-WNS-Type", "wns/raw"}
                            });
            await Notifications.Instance.Hub.SendNotificationAsync(rawNotificationToBeSent, usernameTag);

            // apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync("{\"aps\": {\"content-available\": 1}, \"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}", usernameTag);

            // gcm
            await Notifications.Instance.Hub.SendGcmNativeNotificationAsync("{\"data\": {\"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}}", usernameTag);


            return Request.CreateResponse(HttpStatusCode.OK);
        }


请注意，`Post`方法现在不发送 toast 通知。 它会发送包含仅通知 ID，并没有任何敏感内容的原始通知。 此外，请确保发表评论的平台，您没有凭据配置通知网络集线器，因为它们会导致错误的发送操作。

21. 现在我们将重新部署到 Azure 网站此应用程序，才能使其可从所有设备访问。 在**AppBackend**项目上右键单击，然后选择**发布**。

24. 选择 Azure 网站作为发布目标。 Azure 的帐户登录并选择一个现有的或新的网站，并记**目标 URL**属性的**连接**选项卡中。 我们将在本教程后面部分到此 URL 引用作为您的*后端终结点*。 单击**发布**。