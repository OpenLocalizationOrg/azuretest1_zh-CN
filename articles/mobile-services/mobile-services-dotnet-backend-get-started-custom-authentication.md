---
ms.openlocfilehash: 1238c66bcb439a0b7dc9df18f5c14e0cac109ef7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="开始使用自定义身份验证 |Microsoft Azure" 
    description="了解如何使用用户名和密码的用户进行身份验证。" 
    documentationCenter="Mobile" 
    authors="mattchenderson" 
    manager="dwrede" 
    editor="" 
    services="mobile-services"/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="06/09/2015" 
    ms.author="mahender"/>

# 开始使用自定义身份验证

## 概述
本主题演示如何通过发出自己移动服务身份验证令牌在 Azure 移动服务.NET 后端中的用户进行身份验证。 在本教程中，您添加到快速启动项目为您的应用程序中使用自定义的用户名和密码身份验证。

>[AZURE.NOTE] 本教程演示了一种高级的方法对移动服务的自定义凭据进行身份验证。 许多应用程序都将改为使用内置的社会身份提供程序，允许用户登录 Facebook、 Twitter、 Google、 Microsoft 帐户和 Azure Active Directory 通过最适合。 如果这是您的第一个体验移动服务中的身份验证，请参阅[添加身份验证为您的应用程序]的教程。

本教程基于移动服务快速入门。 此外第一次必须完成本教程[开始使用移动服务]。 

>[AZURE.IMPORTANT] 本教程的目的是向您展示如何为移动服务发出一个身份验证标记。 这并不是作为安全指南。 在开发您的应用程序，您需要知道的密码存储的安全问题，您需要具有管理受到暴力攻击的策略。

## 设置帐户表

由于您正在使用自定义身份验证，并且不依赖于另一种身份标识提供程序，您将需要存储用户的登录信息。 在此部分中，将为帐户创建表格并设置了基本的安全机制。 帐户表会包含用户名和 salted 密码和哈希密码，并且如果需要您还可以包括其他用户信息。

1. 在后端项目的**DataObjects**文件夹中，将添加新实体名为`Account`。

2. 添加以下`using`语句︰

        using Microsoft.WindowsAzure.Mobile.Service;  

3. 类定义替换为以下代码︰

        public class Account : EntityData
        {
            public string Username { get; set; }
            public byte[] Salt { get; set; }
            public byte[] SaltedAndHashedPassword { get; set; }
        }
    
    这代表了一个新的帐户表，其中包含用户名，该用户的盐中的行，securly 存储密码。

2. 在**模型**文件夹中，您将找到**DbContext**派生的类命名您的移动服务。 打开您的上下文和帐户表添加到数据模型，通过包括以下︰

        public DbSet<Account> Accounts { get; set; }

    >[AZURE.NOTE]在本教程中使用的代码段`todoContext`为上下文名称。 您必须更新您的项目上下文的代码段。 
        &nbsp;
    接下来，您将设置为使用此数据的安全功能。 
 
5. 创建名为的类`CustomLoginProviderUtils`并添加以下`using`语句︰

        using System.Security.Cryptography;

6. 将下面的代码方法添加到新类︰


        public static byte[] hash(string plaintext, byte[] salt)
        {
            SHA512Cng hashFunc = new SHA512Cng();
            byte[] plainBytes = System.Text.Encoding.ASCII.GetBytes(plaintext);
            byte[] toHash = new byte[plainBytes.Length + salt.Length];
            plainBytes.CopyTo(toHash,0);
            salt.CopyTo(toHash, plainBytes.Length);
            return hashFunc.ComputeHash(toHash);
        }

        public static byte[] generateSalt()
        {
            RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider();
            byte[] salt = new byte[256];
            rng.GetBytes(salt);
            return salt;
        }

        public static bool slowEquals(byte[] a, byte[] b)
        {
            int diff = a.Length ^ b.Length;
            for (int i = 0; i < a.Length && i < b.Length; i++)
            {
                diff |= a[i] ^ b[i];
            }
            return diff == 0;
        }

    这允许您生成新的长盐、 增加 salted 的密码哈希的能力并提供安全的比较两个哈希值的方式。 

## 创建注册终结点

此时，您有您开始创建用户帐户所需要的一切。 在本节中，您将设置注册终结点来处理新的注册请求。 这是您将在这里实施新的用户名和密码策略并确保不采取的用户名。 然后安全地将存储到数据库中的用户信息。

1. 创建下面的新类来表示传入的注册尝试︰

        public class RegistrationRequest
        {
            public String username { get; set; }
            public String password { get; set; }
        }

    如果您需要收集和存储在注册过程中的其他信息，您应该在以下位置执行。

2. 在移动服务后端项目中，请右键单击**控制器**，单击**添加**，**控制器**，创建一个新的**Microsoft Azure 移动服务自定义控制器**命名为`CustomRegistrationController`，然后添加以下`using`语句︰

        using Microsoft.WindowsAzure.Mobile.Service.Security;
        using System.Text.RegularExpressions;
        using <my_project_namespace>.DataObjects;
        using <my_project_namespace>.Models;

    在上面的代码中，将占位符替换项目的命名空间。
 
4. 类定义替换为以下代码︰

        [AuthorizeLevel(AuthorizationLevel.Anonymous)]
        public class CustomRegistrationController : ApiController
        {
            public ApiServices Services { get; set; }
    
            // POST api/CustomRegistration
            public HttpResponseMessage Post(RegistrationRequest registrationRequest)
            {
                if (!Regex.IsMatch(registrationRequest.username, "^[a-zA-Z0-9]{4,}$"))
                {
                    return this.Request.CreateResponse(HttpStatusCode.BadRequest, "Invalid username (at least 4 chars, alphanumeric only)");
                }
                else if (registrationRequest.password.Length < 8)
                {
                    return this.Request.CreateResponse(HttpStatusCode.BadRequest, "Invalid password (at least 8 chars required)");
                }
    
                todoContext context = new todoContext();
                Account account = context.Accounts.Where(a => a.Username == registrationRequest.username).SingleOrDefault();
                if (account != null)
                {
                    return this.Request.CreateResponse(HttpStatusCode.BadRequest, "That username already exists.");
                }
                else
                {
                    byte[] salt = CustomLoginProviderUtils.generateSalt();
                    Account newAccount = new Account
                    {
                        Id = Guid.NewGuid().ToString(),
                        Username = registrationRequest.username,
                        Salt = salt,
                        SaltedAndHashedPassword = CustomLoginProviderUtils.hash(registrationRequest.password, salt)
                    };
                    context.Accounts.Add(newAccount);
                    context.SaveChanges();
                    return this.Request.CreateResponse(HttpStatusCode.Created);
                }
            }
        }   

    请记住要将*todoContext*变量替换的项目的**DbContext**名。 请注意此控制器使用以下属性以允许对该终结点的所有通信︰

        [AuthorizeLevel(AuthorizationLevel.Anonymous)]

>[AZURE.IMPORTANT]可以通过任何客户端通过 HTTP 访问该注册终结点。 此服务发布到生产环境之前，您应该实现某种形式的方案来验证登记，例如，SMS 或基于电子邮件的验证。 这有助于防止恶意用户创建虚假登记。     

## 创建 LoginProvider

中移动服务身份验证管线的基本构造之一是**LoginProvider**。 在本节中，您将创建您自己`CustomLoginProvider`。 它不会插入到管道中像内置提供程序，但它将为您提供一些方便的功能。  
如果您使用 visual studio 2013，您也许需要安装`WindowsAzure.MobileServices.Backend.Security`nuget 程序包添加到引用`LoginProvider`类。  

1. 创建一个新类， `CustomLoginProvider`，它从**LoginProvider**，然后添加以下`using`语句︰

        using Microsoft.WindowsAzure.Mobile.Service;
        using Microsoft.WindowsAzure.Mobile.Service.Security;
        using Newtonsoft.Json.Linq;
        using Owin;
        using System.Security.Claims;
 
3. **CustomLoginProvider**类定义替换为以下代码︰

        public class CustomLoginProvider : LoginProvider
        {
            public const string ProviderName = "custom";

            public override string Name
            {
                get { return ProviderName; }
            }

            public CustomLoginProvider(IServiceTokenHandler tokenHandler)
                : base(tokenHandler)
            {
                this.TokenLifetime = new TimeSpan(30, 0, 0, 0);
            }

        }

       If you try to build the project now it will fail. `LoginProvider` has three abstract methods that you need to implement, which you will do later.

2. 创建一个名为的新类`CustomLoginProviderCredentials`在同一个代码文件中。 

        public class CustomLoginProviderCredentials : ProviderCredentials
        {
            public CustomLoginProviderCredentials()
                : base(CustomLoginProvider.ProviderName)
            {
            }
        }

    这表示您的用户信息，即可提供给您通过[GetIdentitiesAsync](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobile.service.security.serviceuser.getidentitiesasync.aspx)后端。 如果要添加自定义声明，请确保在此对象中捕获。

3. 添加下面的抽象方法实现`ConfigureMiddleware`到**CustomLoginProvider**。 

        public override void ConfigureMiddleware(IAppBuilder appBuilder, ServiceSettingsDictionary settings)
        {
            // Not Applicable - used for federated identity flows
            return;
        }

    因为**CustomLoginProvider**没有集成使用身份验证管道未实现此方法。

4. 添加下面的抽象方法实现`ParseCredentials`到**CustomLoginProvider**。 

        public override ProviderCredentials ParseCredentials(JObject serialized)
        {
            if (serialized == null)
            {
                throw new ArgumentNullException("serialized");
            }

            return serialized.ToObject<CustomLoginProviderCredentials>();
        }

    此方法将允许后端进行反序列化从传入的身份验证令牌的用户信息。

5. 添加下面的抽象方法实现`CreateCredentials`到**CustomLoginProvider**。 

        public override ProviderCredentials CreateCredentials(ClaimsIdentity claimsIdentity)
        {
            if (claimsIdentity == null)
            {
                throw new ArgumentNullException("claimsIdentity");
            }

            string username = claimsIdentity.FindFirst(ClaimTypes.NameIdentifier).Value;
            CustomLoginProviderCredentials credentials = new CustomLoginProviderCredentials
            {
                UserId = this.TokenHandler.CreateUserId(this.Name, username)
            };

            return credentials;
        }

    此方法将[ClaimsIdentity]翻译成中，身份验证令牌发放阶段使用一个[ProviderCredentials]对象。 再次，您将想要捕获在此方法中的任何其他索赔。
    
6. **ConfigOptions**创建后，下面这行代码 App_Start 文件夹中打开 WebApiConfig.cs 项目文件︰
        
        options.LoginProviders.Add(typeof(CustomLoginProvider));

    

## 创建注册终结点

接下来，您将创建终结点，您的用户进行登录。 用户名和密码，您将收到根据数据库来检查通过第一个应用用户的盐，哈希密码，并确保传入的值与数据库匹配。 如果是这样，然后可以创建[ClaimsIdentity] ，并将其传递给**CustomLoginProvider**。 客户端应用程序接收的用户 ID 和身份验证令牌的进一步访问到您的移动服务。

1. 在您的移动服务的后端项目，创建以下新`LoginRequest`类︰

        public class LoginRequest
        {
            public String username { get; set; }
            public String password { get; set; }
        }

    此类表示传入的登录尝试。

2. 创建以下新`CustomLoginResult`类︰

        public class CustomLoginResult
        {
            public string UserId { get; set; }
            public string MobileServiceAuthenticationToken { get; set; }
    
        }

    此类表示成功登录使用的用户标识和身份验证令牌。 请注意此类客户端更容易手动登录响应的强类型客户端上具有与 MobileServiceUser 类形状相同。

2. 用鼠标右键单击**控制器**，单击**添加**，**控制器**，创建一个新的**Microsoft Azure 移动服务自定义控制器**命名为`CustomLoginController`，然后添加以下`using`语句︰

        using Microsoft.WindowsAzure.Mobile.Service.Security;
        using System.Security.Claims;
        using <my_project_namespace>.DataObjects;
        using <my_project_namespace>.Models;

3. **CustomLoginController**类定义替换为以下代码︰
 
        [AuthorizeLevel(AuthorizationLevel.Anonymous)]
        public class CustomLoginController : ApiController
        {
            public ApiServices Services { get; set; }
            public IServiceTokenHandler handler { get; set; }
    
            // POST api/CustomLogin
            public HttpResponseMessage Post(LoginRequest loginRequest)
            {
                todoContext context = new todoContext();
                Account account = context.Accounts
                    .Where(a => a.Username == loginRequest.username).SingleOrDefault();
                if (account != null)
                {
                    byte[] incoming = CustomLoginProviderUtils
                        .hash(loginRequest.password, account.Salt);
    
                    if (CustomLoginProviderUtils.slowEquals(incoming, account.SaltedAndHashedPassword))
                    {
                        ClaimsIdentity claimsIdentity = new ClaimsIdentity();
                        claimsIdentity.AddClaim(new Claim(ClaimTypes.NameIdentifier, loginRequest.username));
                        LoginResult loginResult = new CustomLoginProvider(handler)
                            .CreateLoginResult(claimsIdentity, Services.Settings.MasterKey);
                        var customLoginResult = new CustomLoginResult()
                        {
                            UserId = loginResult.User.UserId,
                            MobileServiceAuthenticationToken = loginResult.AuthenticationToken
                        };
                        return this.Request.CreateResponse(HttpStatusCode.OK, customLoginResult);
                    }
                }
                return this.Request.CreateResponse(HttpStatusCode.Unauthorized,
                    "Invalid username or password");
            }
        }

       Remember to replace the *todoContext* variable with the name of your project's **DbContext**. Note that this controller uses the following attribute to allow all traffic to this endpoint:

        [AuthorizeLevel(AuthorizationLevel.Anonymous)]

>[AZURE.IMPORTANT] 您`CustomLoginController`生产的使用也应包含暴力检测策略。 否则您签入的解决方案可能容易受到攻击。 

## 配置为要求身份验证的移动服务

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]


## 测试使用的测试客户端的登录流

在客户端应用程序中，您必须开发一个自定义登录屏幕它采用用户名和密码，并将它们作为 JSON 负载发送到您注册和登录的终结点。 若要完成本教程，您将改为仅使用内置测试客户端的移动服务.NET 后端。

1. 在 Visual Studio 中，用鼠标右键单击移动服务项目中，然后单击**调试**和**启动新实例**。  

    这将启动您的移动服务的后端项目调试的新实例。 该服务成功启动后，您将看到一个起始页，指出**此移动服务已启动并正在运行**。

2. 在服务启动页上，**请出**，依次键入设置密码为**MS_ApplicationKey**应用程序设置在 web.config 文件中使用空的用户名到身份验证对话框。

3. 中的帮助页中，单击**CustomRegistration** ，然后单击**尝试一下**。

    ![][2]

4. 在正文中，示例字符串替换为满足之前指定的条件，其中的用户名和密码，然后单击**发送**。 

    ![][3]

    应该**201/创建**响应。

5. 单击浏览器的后退按钮，并使用相同的用户名和密码，您在上一步中注册的**CustomLogin**端点重复步骤 2 和 3。 

    ![][4]

    您应该收到响应消息正文包含**用户**JSON 对象，包含*用户 Id*和*authenticationToken*，这是由您自定义的身份验证生成的移动服务身份验证令牌。 此标记足以授予客户端应用程序访问到 TodoItem 的终结点。

    使*authenticationToken*值的一个副本。 您将使用此访问受限制的 TodoItem 终结点。

6. 单击浏览器的后退按钮，然后在 API 文档页、 **GetTables**、**尝试此操作**。

7. 在 GET 请求对话框中，单击**标题**旁的 + 号，键入值`X-ZUMO-AUTH`在左边的框中，粘贴在右边的框中，复制的*authenticationToken*值，然后单击**发送**。

    ![](./media/mobile-services-dotnet-backend-get-started-custom-authentication/mobile-services-dotnet-backend-custom-auth-access-endpoint.png) 

    移动服务应授予对该终结点的访问并返回表中的 TodoItems 列表**200/确定**状态。

    ![](./media/mobile-services-dotnet-backend-get-started-custom-authentication/mobile-services-dotnet-backend-custom-auth-access-success.png) 

>[AZURE.IMPORTANT] 如果您选择还发布此移动服务项目对 Azure 用于测试，请记住您的登录和身份验证提供程序将易受攻击。 请确保它们是可以适当地加强或受保护的测试数据不是对您很重要。 使用自定义身份验证方案的安全生产服务之前需要格外注意。

## 使用自定义身份验证从客户端登录

本部分介绍了从客户端获取访问移动服务所需的身份验证令牌访问自定义身份验证终结点所需的步骤。 因为您需要特定的客户端代码取决于您的客户端，提供的指导，这是平台的不可知。

>[AZURE.NOTE] 移动服务客户端库与服务通过 HTTPS 进行通信。 因为此解决方案要求您将以明文形式发送密码，您必须确保使用 HTTPS，当您调用这些终结点使用直接的其余请求。

1. 在您的客户端应用程序允许用户输入一个用户名和密码创建所需的 UI 元素。

2. 在客户端库在**MobileServiceClient**上使用相应的**invokeApi**方法调用**CustomRegistration**终结点，并在邮件正文中传递的运行时提供的用户名和密码。 

    您只需要调用一次**CustomRegistration**终结点来为指定用户创建一个帐户，只要您在帐户表中保留用户登录信息。 有关如何在各种受支持的客户端平台上调用一个自定义的 API 的示例，请参阅文章[自定义 Azure 移动服务--客户端 Sdk 中的 API](http://blogs.msdn.com/b/carlosfigueira/archive/2013/06/19/custom-api-in-azure-mobile-services-client-sdks.aspx)。
     
    > [AZURE.IMPORTANT] 由于此用户供应步骤只发生一次，您应该考虑一些带外方式创建用户帐户。 对于公共注册终结点，还应考虑实现一个基于 SMS 的或基于电子邮件的验证过程或某些其他安全措施，以便防止生成的 fruadulent 帐户。 可以使用 Twilio 来发送 SMS 消息的移动服务。 有关详细信息，请参阅[如何︰ 发送 SMS 消息](partner-twilio-mobile-services-how-to-use-voice-sms.md#howto_send_sms)。 您可以使用 SendGrid 从移动服务发送电子邮件。 多个 inforation，请参阅[发送电子邮件与 SendGrid 的移动服务](store-sendgrid-mobile-services-send-email-scripts.md)。 
    
3. 使用适当的**invokeApi**方法，这一次调用**CustomLogin**终结点，并在邮件正文中传递的运行时提供的用户名和密码。 

    这一次，您必须捕获成功登录之后，在响应对象再次*用户 Id*和*authenticationToken*值。 
    
4. 使用返回*用户 Id*和*authenticationToken*值来创建一个新的**MobileServiceUser**对象并将其设置为您的**MobileServiceClient**实例的当前用户[添加到现有应用程序的身份验证](mobile-services-dotnet-backend-ios-get-started-users.md)的主题中所示。 因为 CustomLogin 结果与**MobileServiceUser**对象的形状相同，您应能够直接强制转换结果。 

完成本教程。 


<!-- Anchors. -->


<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-get-started-custom-authentication/mobile-services-dotnet-backend-debug-start.png
[1]: ./media/mobile-services-dotnet-backend-get-started-custom-authentication/mobile-services-dotnet-backend-try-out.png
[2]: ./media/mobile-services-dotnet-backend-get-started-custom-authentication/mobile-services-dotnet-backend-custom-auth-test-client.png
[3]: ./media/mobile-services-dotnet-backend-get-started-custom-authentication/mobile-services-dotnet-backend-custom-auth-send-register.png
[4]: ./media/mobile-services-dotnet-backend-get-started-custom-authentication/mobile-services-dotnet-backend-custom-auth-login-result.png


<!-- URLs. -->
[添加到您的应用程序的身份验证]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md
[开始使用移动服务]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md

[ClaimsIdentity]: https://msdn.microsoft.com/library/system.security.claims.claimsidentity(v=vs.110).aspx
[ProviderCredentials]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobile.service.security.providercredentials.aspx
 
测试
