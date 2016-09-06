---
ms.openlocfilehash: b131dc5a6d14fbb89db322c1afda5b7b11bda613
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="代表用户访问 SharePoint |Microsoft Azure" 
    description="了解如何使到 SharePoint 代表用户的呼叫" 
    documentationCenter="" 
    authors="mattchenderson" 
    manager="dwrede" 
    editor="" 
    services="mobile-services"/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/08/2015" 
    ms.author="mahender"/>

# 代表用户访问 SharePoint

<div class="dev-onpage-video-clear clearfix">
<div class="dev-onpage-left-content">
<p>本主题演示如何代表当前登录的用户访问 SharePoint Api。</p>
<p>如果您更喜欢观看视频，右侧的剪辑将遵循本教程中的相同步骤。 在视频中，垫 Velloso 指导您完成更新 Windows 应用商店应用程序与 SharePoint Online 进行交互。</p>
</div>
<div class="dev-onpage-video-wrapper"><a href="http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Azure-Mobile-Services-AAD-O365-Authentication-identity-across-services" target="_blank" class="label">观看教程</a> <a style="background-image: url('http://media.ch9.ms/ch9/f217/3f8cbf94-f36b-4162-b3da-1c00339ff217/AzureMobileServicesAADO365AuthenticationIdentityA_960.jpg') !important;" href="http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Azure-Mobile-Services-AAD-O365-Authentication-identity-across-services" target="_blank" class="dev-onpage-video"><span class="icon">播放视频</span></a> <span class="time">12:51</span></div>
</div>

在本教程中，您将活动目录身份验证库 Single Sign-on 教程可以在 SharePoint Online 中创建一个 Word 文档，当添加新的 TodoItem 更新该应用程序从您的应用程序进行身份验证。

本教程将引导您完成以下基本步骤启用 SharePoint 上代表的访问︰

1. [注册到 SharePoint 委派访问权限的应用程序]
2. [将 SharePoint 信息添加到您的移动服务]
3. [获取一个访问令牌，然后调用 SharePoint API]
4. [创建并上载的 Word 文档]
5. [测试应用程序]

本教程要求如下︰

* Visual Studio 2013 年运行在 Windows 8.1
* 有效的[SharePoint Online]订阅
* 完成[身份验证与活动目录身份验证库单一登录应用程序]教程。 应使用由您的 SharePoint 订购租户。

## <a name="configure-permissions"></a>配置 SharePoint 委派访问权限的应用程序
默认情况下，您收到来自 AAD 的标记具有有限的权限。 为了访问第三方资源或如 SharePoint Online 的 SaaS 应用程序，您必须明确允许它。

1. 在[Azure 管理门户网站] **Active Directory**部分中，选择您的租户。 导航到您的移动服务创建的 web 应用程序。

    ![][0]

2. 在**配置**选项卡中向下滚动页面到其他应用程序部分的权限。 选择**Office 365 SharePoint Online**以及**编辑或删除用户的文件**授予委派权限。 然后单击**保存**。

    ![][1]

您现在已经配置 AAD 向移动服务发出一个 SharePoint 访问令牌。

## <a name="store-credentials"></a>将 SharePoint 信息添加到您的移动服务

为了打电话到 SharePoint，您需要指定要与之对话的移动服务的终结点。 您还需要能够证明身份的移动服务。 这是使用客户机 ID 和客户端密钥对。 已获得并 AAD 登录安装期间存储移动服务的客户端 ID。 因为这些敏感的凭据，您不应作为我们的代码以纯文本形式存储它们。 相反，您将设置这些值作为应用程序设置为我们的移动服务。

1. 为您的租户，返回到 AAD 的应用程序选项卡并选择您的移动服务的 web 应用程序。

2. 在配置上下, 键向下滚动。 通过生成一个新密钥，您将获得一个客户机密。 注意一次创建一个密钥并离开某个页面，没有方法来摆脱其门户再次。 在创建时必须复制并保存在安全位置，此值。 选择您的密钥的持续时间，然后单击保存和复制出所得到的值。

    ![][2]

3. 管理门户网站移动服务部分中，导航到配置选项卡，并向下滚动到的应用程序设置。 这里您可以提供一键 / 值对来帮助您引用所需的凭据。

    ![][3]

4. 设置 SP_Authority 的 AAD 租户授权的终结点。 这应该是用于客户端应用程序的授权值相同。 它将为窗体 https://login.windows.net/contoso.onmicrosoft.com

5. 设置 SP_ClientSecret 先前获得的客户机密值。

6. 设置 SP_SharePointURL 为 SharePoint 网站的 URL。 它应该采用窗体 https://contoso-my.sharepoint.com

您将能够获得这些值再次在我们使用 ApiServices.Settings 的代码。

## <a name="obtain-token"></a>获取一个访问令牌，然后调用 SharePoint API

为了访问 SharePoint，目标受众为需要 SharePoint 的特殊访问令牌。 若要获取令牌，将需要重新调入 AAD 的移动服务和已颁发令牌的身份的用户。

1. 在 Visual Studio 中打开您的移动服务后端项目。

[AZURE.INCLUDE [mobile-services-dotnet-adal-install-nuget](../../includes/mobile-services-dotnet-adal-install-nuget.md)]

2. 在您移动服务的后端项目，创建一个名为 SharePointUploadContext 的新类。 中，添加以下代码︰

        private String accessToken;
        private String mySiteApiPath;
        private String clientId;
        private String clientSecret;
        private String sharepointURL;
        private String authority;
        private const string getFilesPath = "/getfolderbyserverrelativeurl('Documents')/Files";

        public static async Task<SharePointUploadContext> createContext(ServiceUser user, ServiceSettingsDictionary settings)
        {
            //Get the current access token
            AzureActiveDirectoryCredentials creds = (await user.GetIdentitiesAsync()).OfType<AzureActiveDirectoryCredentials>().FirstOrDefault();
            string userToken = creds.AccessToken;
            return new SharePointUploadContext(userToken, settings);
        }

        private SharePointUploadContext(string userToken, ServiceSettingsDictionary settings)
        {
            //Call ADAL and request a token to SharePoint with the access token
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = ac.AcquireToken(sharepointURL, new ClientCredential(clientId, clientSecret), new UserAssertion(userToken));
            accessToken = ar.AccessToken;
            string upn = ar.UserInfo.UserId;
            mySiteApiPath = "/personal/" + upn.Replace('@','_').Replace('.','_') + "/_api/web"; 
            clientId = settings.AzureActiveDirectoryClientId;
            clientSecret = settings["SP_ClientSecret"];
            sharepointURL = settings["SP_SharePointURL"];
            authority = settings["SP_Authority"];
        }

3. 现在创建一个方法来将该文件添加到用户的文档库︰

        public async Task<bool> UploadDocument(string docName, byte[] document)
        {
            string uploadUri = sharepointURL + mySiteApiPath + getFilesPath + string.Format(@"/Add(url='{0}.docx', overwrite=true)", docName);
            using (HttpClient client = new HttpClient())
            {
                Func<HttpRequestMessage> requestCreator = () =>
                {
                    HttpRequestMessage UploadRequest = new HttpRequestMessage(HttpMethod.Post, uploadUri);
                    UploadRequest.Content = new System.Net.Http.ByteArrayContent(document);
                    UploadRequest.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                    UploadRequest.Content.Headers.ContentType.Parameters.Add(new NameValueHeaderValue("odata", "verbose"));
                    return UploadRequest;
                };
                using (HttpRequestMessage uploadRequest = requestCreator.Invoke())
                {
                    uploadRequest.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
                    HttpResponseMessage uploadResponse = await client.SendAsync(uploadRequest);
                }
            }
            return true;
        }

## <a name="create-document"></a>创建并上载的 Word 文档

若要创建一个 Word 文档，您将使用 OpenXML NuGet 程序包。 通过打开管理器 NuGet 并搜索 DocumentFormat.OpenXml 安装此包。

1. 将以下代码添加到 TodoItemController 中。 这将创建一个基于 TodoItem 的 Word 文档。 在文档的文本将是项的名称。

        private static byte[] CreateWordDocument(TodoItem todoItem)
        {
            byte[] document;
            using (MemoryStream generatedDocument = new MemoryStream())
            {
                using (WordprocessingDocument package = WordprocessingDocument.Create(generatedDocument, WordprocessingDocumentType.Document))
                {
                    MainDocumentPart mainPart = package.MainDocumentPart;
                    if (mainPart == null)
                    {
                        mainPart = package.AddMainDocumentPart();
                        new Document(new Body()).Save(mainPart);
                    }
                    Body body = mainPart.Document.Body;
                    Paragraph p =
                        new Paragraph(
                            new Run(
                                new Text(todoItem.Text)));
                    body.Append(p);
                    mainPart.Document.Save();
                }
                document = generatedDocument.ToArray();
            }
            return document;
        }

2. 用以下内容替换 PostTodoItem:

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
            
            SharePointUploadContext context = await SharePointUploadContext.createContext((ServiceUser)this.User, Services.Settings);
            byte[] document = CreateWordDocument(item);
            bool uploadResult = await context.UploadDocument(item.Id, document);
            
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

## <a name="test-application"></a>测试应用程序

1. 将更改发布到后端，然后再运行客户端应用程序。 出现提示时，登录，并插入新的 TodoItem。

2. 导航到 SharePoint 网站，并使用同一个用户登录。

3. 选择 OneDrive 选项卡。 在文档文件夹下，您应该看到一个 GUID 标题的 Word 文档。 当您打开它时，您应该为您 TodoItem 找到文本。

    ![][4]


<!-- Images. -->

[0]: ./media/mobile-services-dotnet-backend-calling-sharepoint-on-behalf-of-user/aad-web-application.png
[1]: ./media/mobile-services-dotnet-backend-calling-sharepoint-on-behalf-of-user/aad-sharepoint-permissions.png
[2]: ./media/mobile-services-dotnet-backend-calling-sharepoint-on-behalf-of-user/aad-manage-secret-key.png
[3]: ./media/mobile-services-dotnet-backend-calling-sharepoint-on-behalf-of-user/mobile-services-app-settings-sharepoint.png
[4]: ./media/mobile-services-dotnet-backend-calling-sharepoint-on-behalf-of-user/sharepoint-document-created.png

<!-- Anchors. -->

[注册到 SharePoint 委派访问权限的应用程序]: #configure-permissionss
[将 SharePoint 信息添加到您的移动服务]: #store-credentials
[获取一个访问令牌，然后调用 SharePoint API]: #obtain-token
[创建并上载的 Word 文档]: #create-document
[测试应用程序]: #test-application

<!-- URLs. -->
[Azure 的管理门户]: https://manage.windowsazure.com/
[SharePoint 在线]: http://office.microsoft.com/sharepoint/
[对您的应用程序使用 Active Directory 验证库单一登录进行身份验证]: http://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-adal-sso-authentication/
 
测试
