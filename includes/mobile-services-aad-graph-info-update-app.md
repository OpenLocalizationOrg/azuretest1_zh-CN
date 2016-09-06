---
ms.openlocfilehash: badb545d90eede49460896cc508f09167b1f38a6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


以下说明适用于更新 Windows 应用商店的客户端应用程序，但您可以测试这在任何其他 Azure 移动服务所支持的平台上。 


1. 在 Visual Studio，打开 MainPage.xaml.cs，然后添加以下`using`语句到文件的顶部。
 
        using System.Net.Http;

2. 在 MainPage.xaml.cs，将下面的类定义添加的命名空间，以便序列化的用户信息。

        public class UserInfo
        {
            public String displayName { get; set; }
            public String streetAddress { get; set; }
            public String city { get; set; }
            public String state { get; set; }
            public String postalCode { get; set; }
            public String mail { get; set; }
            public String[] otherMails { get; set; }
            
            public override string ToString()
            {
                return "displayName : " + displayName + "\n" +
                       "streetAddress : " + streetAddress + "\n" +
                       "city : " + city + "\n" +
                       "state : " + state + "\n" +
                       "postalCode : " + postalCode + "\n" +
                       "mail : " + mail + "\n" +
                       "otherMails : " + string.Join(", ", otherMails);
            }
        }


3. 在 MainPage.xaml.cs，更新`AuthenticateAsync`方法调用的自定义的 API，从 AAD 返回有关用户的其他信息。 

        private async System.Threading.Tasks.Task AuthenticateAsync()
        {
            while (user == null)
            {
                string message;
                try
                {
                    // Change 'MobileService' to the name of your MobileServiceClient instance.
                    // Sign-in using Facebook authentication.
                    user = await App.MobileService
                        .LoginAsync(MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory);

                    UserInfo userInfo = await App.MobileService
                        .InvokeApiAsync<UserInfo>("getuserinfo", HttpMethod.Get, null);

                    message = string.Format("User info for the logged in user...\n\n{0}",userInfo.ToString());
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }

                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


4. 保存您的更改并生成该服务以验证没有语法错误。  
