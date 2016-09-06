---
ms.openlocfilehash: 3f444052405ee69723049756b531461859254f46
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="连接到企业 SaaS 的移动应用程序 |Microsoft Azure"
    description="了解如何使企业的资源，如 SharePoint Online 的调用"
    documentationCenter=""
    authors="mattchenderson"
    manager="dwrede"
    editor="na"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="06/19/2015"
    ms.author="mahender"/>

# 将您的移动应用程序连接到 SaaS 的 Api

在本教程中，将您的移动应用程序连接到一个企业级软件作为服务 (SaaS) 解决方案。 将更新该应用程序从 [验证您的应用程序使用 Azure 活动目录身份验证库单一登录] 要在 SharePoint Online 创建 Microsoft Word 文档，添加新的 TodoItem。

本教程要求如下︰

* Visual Studio 2013 年运行在 Windows 8.1
* 有效的[SharePoint Online]订阅
* 完成[身份验证与活动目录身份验证库单一登录应用程序]教程。 应使用由您的 SharePoint 订购租户。

## <a name="configure-permissions"></a>配置 SharePoint 委派访问权限的应用程序
默认情况下，您收到来自 AAD 的标记具有有限的权限。 为了访问第三方资源或如 SharePoint Online 的 SaaS 应用程序，您必须明确允许它。

1. 在[Azure 管理门户网站] **Active Directory**部分中，选择您的租户。 导航到您的应用程序服务创建的 web 应用程序。

2. 在**配置**选项卡中向下滚动页面到其他应用程序部分的权限。 选择**Office 365 SharePoint Online**以及**编辑或删除用户的文件**授予委派权限。 然后单击**保存**。

    ![][1]

您现在已经配置 Azure 将 SharePoint 访问令牌发送到应用程序服务的广告。

## <a name="store-credentials"></a>将 SharePoint 信息添加到您的移动应用程序

为了打电话到 SharePoint，您需要指定要与之对话的移动应用程序的终结点。 您还需要能够证明您的应用程序服务的身份。 这是使用客户机 ID 和客户端密钥对。 已获得并 AAD 登录安装过程中存储应用程序服务的客户端 ID。 因为这些敏感的凭据，您的 should'nt 会将其作为我们的代码以纯文本形式存储。 相反，您将设置这些值作为应用程序设置为我们移动的应用程序代码的网站。

1. 为您的租户，返回到 AAD 的应用程序选项卡并选择您的应用程序服务 web 应用程序。

2. 在配置上下, 键向下滚动。 获得客户端密码通过生成一个新密钥。 注意一次创建一个密钥并离开某个页面，没有方法来摆脱其门户再次。 在创建时必须复制并保存在安全位置，此值。 选择您的密钥的持续时间，然后单击保存和复制出所得到的值。

3. 管理门户网站移动应用程序代码部分中，导航到配置选项卡，并向下滚动到的应用程序设置。 这里您可以提供一键 / 值对来帮助您引用所需的凭据。

* 设置 SP_Authority 的 AAD 租户授权的终结点。 这应该是用于客户端应用程序的授权值相同。 将窗体的 `https://login.windows.net/contoso.onmicrosoft.com`

* 设置 SP_ClientSecret 先前获得的客户机密值。

* 设置 SP_SharePointURL 为 SharePoint 网站的 URL。 它应该是窗体的 `https://contoso-my.sharepoint.com`

您将能够获得再次在代码使用 ApiServices.Settings 中的这些值。

## <a name="obtain-token"></a>获取一个访问令牌，然后调用 SharePoint API

为了访问 SharePoint，目标受众为需要 SharePoint 的特殊访问令牌。 若要获取令牌，您需要到 Azure 应用程序服务和已颁发令牌的身份与 AD 回叫用户。

1. 在 Visual Studio 中打开您的移动应用程序的代码项目。

[AZURE.INCLUDE [app-service-mobile-dotnet-adal-install-nuget](../../includes/app-service-mobile-dotnet-adal-install-nuget.md)]

2. 在 NuGet 程序包管理器中，单击**联机**。 输入**Microsoft.Azure.Mobile.Server.AppService**作为搜索条件。 然后单击**安装**以安装[移动应用程序.NET 后端应用程序服务扩展]包。 此软件包提供扩展方法用于处理当前登录用户的相关信息。

2. 在移动应用程序的代码项目中，创建一个名为 SharePointUploadContext 的新类。 添加`using Microsoft.Azure.Mobile.Server.AppService;`文件的语句。 然后，添加以下类︰

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

<!-- Images. -->

[1]: ./media/app-service-mobile-dotnet-backend-get-started-connect-to-enterprise/aad-sharepoint-permissions.png

<!-- URLs. -->

[预览 Azure 的管理门户]: https://portal.azure.com/
[Azure 的管理门户]: https://manage.windowsazure.com/
[SharePoint 在线]: http://office.microsoft.com/en-us/sharepoint/
[对您的应用程序使用 Active Directory 验证库单一登录进行身份验证]: app-service-mobile-dotnet-backend-ios-aad-sso-preview.md
[移动应用程序.NET 后端应用程序服务扩展]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.AppService/

测试
