---
ms.openlocfilehash: d52baa92277d89c248c67cda3ba7456580dfe607
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="基于角色的访问控制，在移动服务和 Azure Active Directory （Windows 应用商店） |Microsoft Azure"
    description="了解如何控制访问基于 Azure Active Directory Windows 应用商店应用程序中的角色。"
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

# 基于角色的移动服务和 Azure 的 Active Directory 中的访问控制

[AZURE.INCLUDE [mobile-services-selector-rbac](../../includes/mobile-services-selector-rbac.md)]

##概述

基于角色的访问控制 (RBAC) 是一种将权限分配给角色可容纳用户的行为。 它可以很好地定义边界上某些级别的用户可以做什么和不能做什么。 本教程将指导您完成如何将 RBAC 基本添加到 Azure 移动服务。

本教程将说明基于角色的访问控制，检查每个用户定义 Azure 活动目录 (AAD) 中的销售组的成员身份。 与.NET 移动服务后端使用 Azure Active Directory[图 REST API]进行访问检查。 只有属于销售组的用户可以查询的数据。


>[AZURE.NOTE] 本教程的目的是扩展身份验证包括授权做法的知识。 应首先完成使用 Azure Active Directory 身份验证提供程序[将身份验证添加到您的应用程序]教程。 本教程将继续更新 TodoItem 应用程序[添加到您的应用程序的身份验证]本教程中使用。

##先决条件

本教程要求如下︰

* Visual Studio 2013 年 Windows 8.1 上运行。
* 完成[添加到您的应用程序的身份验证]本教程使用 Azure Active Directory 身份验证提供程序。




##为集成的应用程序生成密钥


期间[将添加到您的应用程序的身份验证]教程中，您创建集成的应用程序注册完成[注册使用 Azure 活动目录登录]步骤。 在本节中，您生成密钥用于读取目录信息的集成应用程序的客户端 id。

如果您有机会去[访问 Azure 活动目录图表信息]本教程，您已经完成了这一步，可跳过本节。

[AZURE.INCLUDE [mobile-services-generate-aad-app-registration-access-key](../../includes/mobile-services-generate-aad-app-registration-access-key.md)]



##销售组创建具有成员资格

[AZURE.INCLUDE [mobile-services-aad-rbac-create-sales-group](../../includes/mobile-services-aad-rbac-create-sales-group.md)]



##移动服务上创建一个自定义的授权特性

在本节中，您将创建新的自定义授权属性可以用于执行访问权限检查对移动服务运营。 该特性将查找 Active Directory 组根据传递给它的角色名称。 然后，它会执行访问权限检查根据该组的成员身份。

1. 在 Visual Studio 中，右键单击移动服务的后端.NET 项目，然后单击**管理 NuGet 程序包**。

2. 在程序包管理器 NuGet 对话框中，输入搜索条件以查找并安装您的移动服务在**活动目录身份验证库** **ADAL** 。 本教程是最近经 ADAL 软件包的 3.3.205061641-alpha （预发布） 版本。

3. 在 Visual Studio 中，右键单击您的移动服务项目并单击**添加**，然后**新的文件夹**。 命名新文件夹**实用程序**。

4. 在 Visual Studio 中，右键单击新的**实用程序**文件夹并添加一个名为**AuthorizeAadRole.cs**的新类文件。

    ![][0]

5. 在 AuthorizeAadRole.cs 文件中，添加以下`using`语句在文件的顶部。

        using System.Net;
        using System.Net.Http;
        using System.Web.Http;
        using System.Web.Http.Controllers;
        using System.Web.Http.Filters;
        using Newtonsoft.Json;
        using Microsoft.WindowsAzure.Mobile.Service.Security;
        using Microsoft.WindowsAzure.Mobile.Service;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using System.Globalization;
        using System.IO;

6. 在 AuthorizeAadRole.cs，对实用程序名称空间中添加下面的枚举的类型。 在此示例中我们只处理了**销售代表**角色。 其他人都只是属于组，则可以使用。

        public enum AadRoles
        {
            Sales,
            Management,
            Development
        }

7. 在 AuthorizeAadRole.cs 中，添加以下`AuthorizeAadRole`类实用程序命名空间定义。

        [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = false, Inherited = true)]
        public class AuthorizeAadRole : AuthorizationFilterAttribute
        {
            private bool isInitialized;
            private bool isHosted;
            private ApiServices services = null;

            // Constants used with ADAL and the Graph REST API for AAD
            private const string AadInstance = "https://login.windows.net/{0}";
            private const string GraphResourceId = "https://graph.windows.net/";
            private const string APIVersion = "?api-version=2013-04-05";

            // App settings pulled from the Mobile Service
            private string tenantdomain;
            private string clientid;
            private string clientkey;
            private Dictionary<int, string> groupIds = new Dictionary<int, string>();

            private string token = null;

            public AuthorizeAadRole(AadRoles role)
            {
                this.Role = role;
            }

            // private class used to serialize the Graph REST API web response
            private class MembershipResponse
            {
                public bool value;
            }

            public AadRoles Role { get; private set; }

            // Generate a local dictionary for the role group ids configured as
            // Mobile Service app settings
            private void InitGroupIds()
            {
            }

            // Use ADAL and the authentication app settings from the Mobile Service to
            // get an AAD access token
            private string GetAADToken()
            {
            }

            // Given an AAD user id, check membership against the group associated with the role.
            private bool CheckMembership(string memberId)
            {
            }

            // Called when the user is attempting authorization
            public override void OnAuthorization(HttpActionContext actionContext)
            {
            }
        }



8. 在 AuthorizeAadRole.cs，更新`InitGroupIds`方法在`AuthorizeAadRole`类，如下所示。 此方法创建字典给每个角色组 id 的映射。

        private void InitGroupIds()
        {
            string groupId;

            if (services == null)
                return;

            if (!groupIds.ContainsKey((int)AadRoles.Sales))
            {
                if (services.Settings.TryGetValue("AAD_SALES_GROUP_ID", out groupId))
                {
                    groupIds.Add((int)AadRoles.Sales, groupId);
                }
                else
                    services.Log.Error("AAD_SALES_GROUP_ID app setting not found.");
            }
        }


9. 在 AuthorizeAadRole.cs，更新`GetAADToken`方法在`AuthorizeAadRole`类。 此方法使用移动服务中存储的应用程序设置来获得 ADAL 访问令牌 AAD。

    >[AZURE.NOTE] ADAL.net 包括内存中令牌缓存默认情况下，为了帮助缓解与活动目录的额外网络流量。 但是，您可以编写自己的缓存实现或禁用缓存完全。 有关更多信息，请参见[.net ADAL]。

        // Use ADAL and the authentication app settings from the Mobile Service to get an AAD access token
        private async Task<string> GetAADToken()
        {
            // Try to get the required AAD authentication app settings from the mobile service.  
            if (!(services.Settings.TryGetValue("AAD_CLIENT_ID", out clientid) &
                  services.Settings.TryGetValue("AAD_CLIENT_KEY", out clientkey) &
                  services.Settings.TryGetValue("AAD_TENANT_DOMAIN", out tenantdomain)))
            {
                services.Log.Error("GetAADToken() : Could not retrieve mobile service app settings.");
                return null;
            }

            ClientCredential clientCred = new ClientCredential(clientid, clientkey);
            string authority = String.Format(CultureInfo.InvariantCulture, AadInstance, tenantdomain);
            AuthenticationContext authContext = new AuthenticationContext(authority);

            AuthenticationResult result = await authContext.AcquireTokenAsync(GraphResourceId, clientCred);
            if (result != null)
                token = result.AccessToken;
            else
                services.Log.Error("GetAADToken() : Failed to return a token.");

            return token;
        }

10. 在 AuthorizeAadRole.cs，更新`CheckMembership`方法在`AuthorizeAadRole`类。 此方法接收用户的对象 id。 然后使用 AAD 图 REST API，请检查该对象 id 是否与上所选的角色相关联的组的成员 id`AuthorizeAadRole`类

        private bool CheckMembership(string memberId)
        {
            bool membership = false;
            string url = GraphResourceId + tenantdomain + "/isMemberOf" + APIVersion;
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(url);

            // Use the Graph REST API to check group membership in the AAD
            try
            {
                request.Method = "POST";
                request.ContentType = "application/json";
                request.Headers.Add("Authorization", token);
                using (var sw = new StreamWriter(request.GetRequestStream()))
                {
                    // Request body must have the group id and a member id to check for membership
                    string body = String.Format("\"groupId\":\"{0}\",\"memberId\":\"{1}\"",
                        groupIds[(int)Role], memberId);
                    sw.Write("{" + body + "}");
                }

                WebResponse response = request.GetResponse();
                StreamReader sr = new StreamReader(response.GetResponseStream());
                string json = sr.ReadToEnd();
                MembershipResponse membershipResponse = JsonConvert.DeserializeObject<MembershipResponse>(json);
                membership = membershipResponse.value;
            }
            catch (Exception e)
            {
                services.Log.Error("OnAuthorization() exception : " + e.Message);
            }

            return membership;
        }


11. 在 AuthorizeAadRole.cs，更新`OnAuthorization`在`AuthorizeAadRole`用以下代码的类。 此代码要求与 AAD 调入 Mobiile 服务的用户已通过身份验证。  然后，获取用户的 AAD 对象 id，并检查与 Active Directory 组对应于该角色的成员资格。

    >[AZURE.NOTE] 您可以按名称查找 Active Directory 组。 但是，在很多情况下它是更好的做法作为手机信息服务的应用程序设置中存储的组 id。 这是因为组名称是有可能会改变，但 id 保持不变。

        public override void OnAuthorization(HttpActionContext actionContext)
        {
            if (actionContext == null)
            {
                throw new ArgumentNullException("actionContext");
            }

            services = new ApiServices(actionContext.ControllerContext.Configuration);

            // Check whether we are running in a mode where local host access is allowed
            // through without authentication.
            if (!this.isInitialized)
            {
                HttpConfiguration config = actionContext.ControllerContext.Configuration;
                this.isHosted = config.GetIsHosted();
                this.isInitialized = true;
            }

            // No security when hosted locally
            if (!this.isHosted && actionContext.RequestContext.IsLocal)
            {
                services.Log.Warn("AuthorizeAadRole: Local Hosting.");
                return;
            }

            ApiController controller = actionContext.ControllerContext.Controller as ApiController;
            if (controller == null)
            {
                services.Log.Error("AuthorizeAadRole: No ApiController.");
            }

            bool isAuthorized = false;
            try
            {
                // Initialize a mapping for the group id to our enumerated type
                InitGroupIds();

                // Retrieve a AAD token from ADAL
                GetAADToken();
                if (token == null)
                {
                    services.Log.Error("AuthorizeAadRole: Failed to get an AAD access token.");
                }
                else
                {
                    // Check group membership to see if the user is part of the group that corresponds to the role
                    if (!string.IsNullOrEmpty(groupIds[(int)Role]))
                    {
                        ServiceUser serviceUser = controller.User as ServiceUser;
                        if (serviceUser != null && serviceUser.Level == AuthorizationLevel.User)
                        {
                            var idents = serviceUser.GetIdentitiesAsync().Result;
                            AzureActiveDirectoryCredentials clientAadCredentials =
                                idents.OfType<AzureActiveDirectoryCredentials>().FirstOrDefault();
                            if (clientAadCredentials != null)
                            {
                                isAuthorized = CheckMembership(clientAadCredentials.ObjectId);
                            }
                        }
                    }
                }
            }
            catch (Exception e)
            {
                services.Log.Error(e.Message);
            }
            finally
            {
                if (isAuthorized == false)
                {
                    services.Log.Error("Denying access");

                    actionContext.Response = actionContext.Request
                        .CreateErrorResponse(HttpStatusCode.Forbidden,
                            "User is not logged in or not a member of the required group");
                }
            }
        }

12. 将您的更改保存到 AuthorizeAadRole.cs。

##添加基于角色的访问检查到数据库操作

1. 在 Visual Studio 中，展开移动服务项目下的**控制器**文件夹。 打开 TodoItemController.cs。

2. 在 TodoItemController.cs，添加`using`实用程序名称空间包含自定义的授权属性的语句。

        using todolistService.Utilities;

3. 在 TodoItemController.cs，可以添加到控制器类或个别的方法，具体取决于您希望 access 检查该特性。 如果您希望所有的控制器操作来检查访问权限基于角色相同的角色，只向类中添加属性。 将属性添加到类，如下所示的测试本教程。

        [AuthorizeAadRole(AadGroups.Sales)]
        public class TodoItemController : TableController<TodoItem>

    如果您只希望对访问检查插入、 更新和删除操作，将在如下只在那些方法上设置的属性。

        // PATCH tables/TodoItem
        [AuthorizeAadRole(AadGroups.Sales)]
        public Task<TodoItem> PatchTodoItem(string id, Delta<TodoItem> patch)
        {
            return UpdateAsync(id, patch);
        }

        // POST tables/TodoItem
        [AuthorizeAadRole(AadGroups.Sales)]
        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

        // DELETE tables/TodoItem
        [AuthorizeAadRole(AadGroups.Sales)]
        public Task DeleteTodoItem(string id)
        {
            return DeleteAsync(id);
        }


4. 保存 TodoItemController.cs 并生成移动的服务，以确认没有语法错误。
5. 发布到 Azure 帐户移动服务。


##测试客户端的访问

[AZURE.INCLUDE [mobile-services-aad-rbac-test-app](../../includes/mobile-services-aad-rbac-test-app.md)]







<!-- Images -->
[0]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-aad-rbac/add-authorize-aad-role-class.png

<!-- URLs. -->
[添加到您的应用程序的身份验证]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md
[如何在 Azure 的 Active Directory 中注册]: mobile-services-how-to-register-active-directory-authentication.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[目录同步方案]: http://msdn.microsoft.com/library/azure/jj573653.aspx
[存储服务器脚本]: mobile-services-store-scripts-source-control.md
[注册使用 Azure 活动目录登录]: mobile-services-how-to-register-active-directory-authentication.md
[REST API，关系图]: http://msdn.microsoft.com/library/azure/hh974478.aspx
[IsMemberOf]: http://msdn.microsoft.com/library/azure/dn151601.aspx
[访问 Azure Active Directory 图表信息]: mobile-services-dotnet-backend-windows-store-dotnet-aad-graph-info.md
[为使.NET ADAL]: https://msdn.microsoft.com/library/azure/jj573266.aspx

测试
