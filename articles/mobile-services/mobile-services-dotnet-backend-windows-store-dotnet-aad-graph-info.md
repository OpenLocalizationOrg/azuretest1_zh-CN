---
ms.openlocfilehash: aeabfb24afcfaf46c9f82ca076b5330c3a5127d9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="访问 Azure Active Directory 图形信息 （Windows 应用商店） |Microsoft Azure"
    description="了解如何访问 Windows 应用商店应用程序中使用图形 API 的 Azure Active Directory 信息。"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="wesmc"/>

# 访问 Azure Active Directory 图表信息



[AZURE.INCLUDE [mobile-services-selector-aad-graph](../../includes/mobile-services-selector-aad-graph.md)]

##概述

像其他标识提供程序提供移动服务，Azure 活动目录 (AAD) 提供程序还支持一个丰富的图形 API，可以用于以编程方式访问该目录。 在本教程中，您更新任务列表应用程序进行个性化设置返回附加的用户信息从使用[REST API，图表]目录中检索通过身份验证的用户的应用程序体验。

Azure 广告图形 API 的详细信息，请参阅[Azure 活动目录图表团队博客]。


>[AZURE.NOTE] 本教程的目的是扩展身份验证使用 Azure Active Directory 的知识。 预计完成使用 Azure Active Directory 身份验证提供程序[将身份验证添加到您的应用程序]教程。 本教程将继续更新 TodoItem 应用程序[添加到您的应用程序的身份验证]本教程中使用。




##先决条件

在开始本教程之前，您必须已经完成这些移动服务指南︰

+ [添加到您的应用程序的身份验证]<br/>应做事项列表示例应用程序中添加登录要求。

+ [自定义 API 教程]<br/>演示如何调用一个自定义的 API。



## <a name="generate-key"></a>在 AAD 中生成的应用程序注册为访问键


期间[将添加到您的应用程序的身份验证]教程中，您创建集成的应用程序注册完成[注册使用 Azure 活动目录登录]步骤。 在本节中，您生成密钥用于读取目录信息的集成应用程序的客户端 id。

[AZURE.INCLUDE [mobile-services-generate-aad-app-registration-access-key](../../includes/mobile-services-generate-aad-app-registration-access-key.md)]


## <a name="create-api"></a>创建自定义的 GetUserInfo 的 API

在本节中，您将创建将使用 Azure 广告图形 API 检索有关用户的其他信息从 AAD 的 GetUserInfo 自定义 API。

如果您从未使用过自定义的 Api 的移动服务，请完成此部分之前的[自定义 API 教程]。

1. 在 Visual Studio 中，右键单击移动服务的后端.NET 项目，然后单击**管理 NuGet 程序包**。
2. 在程序包管理器 NuGet 对话框中，输入搜索条件以查找并安装您的移动服务在**活动目录身份验证库** **ADAL** 。 本教程是最近经 ADAL 软件包的 3.3.205061641-alpha （预发布） 版本。


3. 在 Visual Studio 中，右键单击移动服务项目的**控制器**文件夹，然后单击**添加**以添加一个新的**Microsoft Azure 移动服务自定义控制器**命名为`GetUserInfoController`。 客户机将调用此 API 来获取在 Active Directory 中的用户信息。

4. 在新的 GetUserInfoController.cs 文件中，添加以下`using`语句。

        using Microsoft.WindowsAzure.Mobile.Service.Security;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using System.Globalization;
        using System.Threading.Tasks;
        using Newtonsoft.Json;
        using System.IO;

5. 在新的 GetUserInfoController.cs 文件中，添加以下`UserInfo`类来保存我们要从 AAD 收集的信息。

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
                       "otherMails : " + string.Join(", ",otherMails);
            }
        }

6. 在 GetUserInfoController.cs 中，添加以下成员变量到`GetUserInfoController`类。

        private const string AadInstance = "https://login.windows.net/{0}";
        private const string GraphResourceId = "https://graph.windows.net/";
        private const string APIVersion = "?api-version=2013-04-05";

        private string tenantdomain;
        private string clientid;
        private string clientkey;
        private string token = null;


7. 在 GetUserInfoController.cs 中，添加以下`GetAADToken`于类的方法。

        private async Task<string> GetAADToken()
        {
            // Try to get the AAD app settings from the mobile service.  
            if (!(Services.Settings.TryGetValue("AAD_CLIENT_ID", out clientid) &
                  Services.Settings.TryGetValue("AAD_CLIENT_KEY", out clientkey) &
                  Services.Settings.TryGetValue("AAD_TENANT_DOMAIN", out tenantdomain)))
            {
                Services.Log.Error("GetAADToken() : Could not retrieve mobile service app settings.");
                return null;
            }

            ClientCredential clientCred = new ClientCredential(clientid, clientkey);
            string authority = String.Format(CultureInfo.InvariantCulture, AadInstance, tenantdomain);
            AuthenticationContext authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(GraphResourceId, clientCred);

            if (result != null)
                token = result.AccessToken;
            else
                Services.Log.Error("GetAADToken() : Failed to return a token.");

            return token;
        }

    此方法使用[Azure 管理门户]以获取令牌来访问 Active Directory 中移动服务上配置的应用程序设置。

7. 在 GetUserInfoController.cs 中，添加以下`GetAADUser`于类的方法。

        private async Task<UserInfo> GetAADUser()
        {
            ServiceUser serviceUser = (ServiceUser)this.User;

            // Need a user
            if (serviceUser == null || serviceUser.Level != AuthorizationLevel.User)
            {
                Services.Log.Error("GetAADUser() : No ServiceUser or wrong Authorizationlevel");
                return null;
            }

            // Get the user's AAD object id
            var idents = serviceUser.GetIdentitiesAsync().Result;
            var clientAadCredentials = idents.OfType<AzureActiveDirectoryCredentials>().FirstOrDefault();
            if (clientAadCredentials == null)
            {
                Services.Log.Error("GetAADUser() : Could not get AAD credientials for the logged in user.");
                return null;
            }

            if (token == null)
                await GetAADToken();

            if (token == null)
            {
                Services.Log.Error("GetAADUser() : No token.");
                return null;
            }

            // User the AAD Graph REST API to get the user's information
            string url = GraphResourceId + tenantdomain + "/users/" + clientAadCredentials.ObjectId + APIVersion;
            Services.Log.Info("GetAADUser() : Request URL : " + url);
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(url);
            request.Method = "GET";
            request.ContentType = "application/json";
            request.Headers.Add("Authorization", token);
            UserInfo userinfo = null;
            try
            {
                WebResponse response = await request.GetResponseAsync();
                StreamReader sr = new StreamReader(response.GetResponseStream());
                string userjson = sr.ReadToEnd();
                userinfo = JsonConvert.DeserializeObject<UserInfo>(userjson);
                Services.Log.Info("GetAADUser user : " + userinfo.ToString());
            }
            catch(Exception e)
            {
                Services.Log.Error("GetAADUser exception : " + e.Message);
            }

            return userinfo;
        }

    此方法对经过授权的用户获取 Active Directory 对象 id，然后使用图形 REST API，从活动目录中获取用户的信息。


8. 在 GetUserInfoController.cs 替换`Get`方法使用下面的方法。 此方法返回从使用图形 REST API，Azure Active directory 用户实体，需要授权的用户调用 API。

        // GET api/GetUserInfo
        [AuthorizeLevel(AuthorizationLevel.User)]
        public async Task<UserInfo> Get()
        {
            Services.Log.Info("Entered GetUserInfo custom controller!");
            return await GetAADUser();
        }

9. 保存您的更改并生成该服务以验证没有语法错误。
10. 发布到 Azure 帐户的移动服务项目。


## <a name="update-app"></a>更新应用程序使用 GetUserInfo

在本节中，您将更新`AuthenticateAsync`您在调用自定义的 API，并从 AAD 返回有关用户的其他信息[将添加到您的应用程序的身份验证]教程中实现的方法。

[AZURE.INCLUDE [mobile-services-aad-graph-info-update-app](../../includes/mobile-services-aad-graph-info-update-app.md)]



## <a name="test-app"></a>测试应用程序

[AZURE.INCLUDE [mobile-services-aad-graph-info-test-app](../../includes/mobile-services-aad-graph-info-test-app.md)]





##<a name="next-steps"></a>下一步行动

在下一步的教程中，[基于角色的访问控制与中移动服务 AAD]，您将使用基于角色的访问控制 Azure 活动目录 (AAD) 的才允许访问检查组成员资格。



<!-- Anchors. -->
[在 AAD 中生成的应用程序注册为访问键]: #generate-key
[创建自定义的 GetUserInfo 的 API]: #create-api
[更新应用程序以使用自定义的 API]: #update-app
[测试应用程序]: #test-app
[下一步行动]:#next-steps

<!-- Images -->


<!-- URLs. -->
[添加到您的应用程序的身份验证]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md
[如何在 Azure 的 Active Directory 中注册]: mobile-services-how-to-register-active-directory-authentication.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[REST API，关系图]: http://msdn.microsoft.com/library/azure/hh974478.aspx
[自定义 API 教程]: mobile-services-dotnet-backend-windows-store-dotnet-call-custom-api.md
[存储服务器脚本]: mobile-services-store-scripts-source-control.md
[注册使用 Azure 活动目录登录]: mobile-services-how-to-register-active-directory-authentication.md
[Azure 的 Active Directory 图团队博客]: http://go.microsoft.com/fwlink/?LinkId=510536
[获取用户]: http://msdn.microsoft.com/library/azure/dn151678.aspx
[基于角色的访问控制与中移动服务 AAD]: mobile-services-dotnet-backend-windows-store-dotnet-aad-rbac.md

测试
