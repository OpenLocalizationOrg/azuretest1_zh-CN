---
ms.openlocfilehash: d89d3f22eb98f8f972021beb80bc42fc173c225d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Azure 密钥存储库，从 Web 应用程序 |Microsoft Azure" 
    description="使用本教程来帮助您学习如何使用 Azure 密钥存储库中的 web 应用程序。" 
    services="key-vault" 
    documentationCenter="" 
    authors="adamhurwitz" 
    manager=""
    tags="azure-resource-manager"//>

<tags 
    ms.service="key-vault" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/24/2015" 
    ms.author="adhurwit"/>

# 使用 Web 应用程序从 Azure 密钥存储库 #

## 简介  
使用本教程来帮助您了解如何从 Azure 中的 web 应用程序中使用 Azure 密钥存储库。 它将引导您完成从 Azure 密钥存储库访问保密，以便可以在 web 应用程序中使用它的过程。

**估计完成时间︰** 15 分钟


关于 Azure 密钥存储库的概述信息，请参阅[Azure 密钥存储库是什么？](key-vault-whatis.md)

## 先决条件 

若要完成本教程，您必须︰

- 一个 URI 到 Azure 密钥存储库中的一个秘密
- 客户机 ID 和客户机密的 web 应用程序中注册 Azure Active Directory 访问您的密钥存储库
- Web 应用程序。 我们将展示在 Azure 作为 Web 应用程序部署的 ASP.NET MVC 应用程序的步骤。 

> [AZURE.NOTE]  完成中列出的步骤[开始使用 Azure 密钥存储库](key-vault-get-started.md)对于本教程，以便 web 应用程序中有一个秘密的客户机 ID 和客户端密钥的 URI 至关重要。 

Web 应用程序要访问密钥存储库是在 Azure Active Directory 中注册，因此已被授予访问您的密钥存储库。 如果不是这种情况，请返回到注册入门教程中的应用程序并重复列出的步骤。 

本教程是为了解基础知识上 Azure 创建 web 应用程序的 web 开发人员设计的。 有关 Azure Web 应用程序的详细信息，请参阅[Web 应用程序概述](../app-service-web-overview.md)。



## <a id="packages"></a>添加 Nuget 程序包 ##
有三个 web 应用程序必须已安装的包。 

- 活动目录身份验证库-包含交互 Azure Active Directory 和管理用户标识的方法
- Azure 键保险存储库 — 包含与 Azure 密钥存储库进行交互的方法


可以使用程序包管理器控制台使用安装软件包命令安装这些软件包的所有三个。 

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault 


## <a id="webconfig"></a>修改 Web.Config ##
有三种，如下所示添加到 web.config 文件的应用程序设置。 

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


如果您不打算主持 Azure Web 应用程序作为您的应用程序，应将实际的客户机 Id、 客户端密码和机密的 URI 值添加到 web.config。 因为我们将附加的安全级别 Azure 门户中添加实际的值，否则将这些虚拟值。 


## <a id="gettoken"></a>添加方法以获取访问令牌 ##
要使用密钥存储库 API 需要一个访问令牌。 密钥存储库客户端处理对密钥存储库 API 调用，但您需要其提供获取访问令牌函数。  

以下是代码以从 Azure Active Directory 中获取访问令牌。 此代码可以在您的应用程序中的任意位置。 我想添加一个 Utils 或 EncryptionHelper 类。  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Web.Configuration;
    
    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);
        
        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");
        
        return result.AccessToken;
    }

> [AZURE.NOTE] 使用客户端密钥使用客户机 ID 和客户端密钥是最简单的方法来鉴别 Azure AD 应用程序。 并在 web 应用程序中使用它允许进行责任区分和更好地控制您的密钥管理。 但它依赖将客户端密钥放在您的配置设置的某些可能会放想要保护在您配置的设置中的秘密，很危险。 有关如何使用客户机 ID 和证书而不是客户机 ID 和客户端密钥来验证 Azure AD 应用程序的讨论，请参阅下面。 



## <a id="appstart"></a>检索应用程序开始的秘密 ##
现在我们需要代码来调用密钥存储库 API 并检索机密。 下面的代码可放任何地方，只要您需要使用它之前调用它。 我已将此代码放在应用程序启动事件在 Global.asax 以便它开始运行一次，使密钥可用于应用程序。 

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method. 
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;
    
    //I put a variable in a Utils class to hold the secret for general  application use. 
    Utils.EncryptSecret = sec;



## <a id="portalsettings"></a>在 Azure 门户 （可选） 添加应用程序设置 ##
如果您有一个 Azure Web 应用程序现在可以在 Azure 门户 AppSettings 为添加实际值。 这样，实际值将不会在 web.config 但通过门户上拥有独立的访问控制功能保护。 这些值将替换为您在 web.config 中输入的值。 请确保名称都相同。

![在 Azure 门户网站中显示的应用程序设置][1]


## 而不是客户端密钥的证书进行身份验证 
另一种方法来鉴别 Azure AD 应用程序是通过使用客户机 ID 和证书而不客户机 ID 和客户的机密。 在 Azure Web 应用程序中使用的证书的步骤如下︰

1. 获取或创建一个证书
2. 将证书与 AD Azure 应用程序相关联
3. 将代码添加到您的 Web 应用程序要使用的证书
4. 将证书添加到您的 Web 应用程序


**获取或创建一个证书**我们的目的而言，我们将测试证书。 以下是几个可用于在开发人员命令提示符创建证书的命令。 将目录更改为您想要创建证书文件。 

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

记的结束日期和.pfx 的密码 (在此示例中︰ 07/31/2016年和 test123)。 您将需要它们下面。 

创建一个测试证书的详细信息，请参阅[如何︰ 创建您的自己测试证书](https://msdn.microsoft.com/en-in/library/ff699202.aspx)


**将 Azure 广告应用程序与该证书相关联**既然您已经有一个证书，您需要与 AD Azure 应用程序。 但 Azure 管理门户不支持此稍后再试。 相反，您需要使用 Powershell。 以下是您需要运行的命令︰

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    
    PS C:\> $x509.Import("C:\data\KVWebApp.cer")
    
    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())
    
    PS C:\> $now = [System.DateTime]::Now
    
    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31") 
    
    PS C:\> $adapp = New-AzureADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow
    
    PS C:\> $sp = New-AzureADServicePrincipal -ApplicationId $adapp.ApplicationId

在运行这些命令之后，您可以看到在 Azure 广告中的应用程序。 如果看不到的"我的公司拥有应用程序"的第一次，搜索应用程序而不是"应用程序公司使用"。 

若要了解有关 Azure AD 的应用程序对象和 ServicePrincipal 对象的详细信息，请参见[应用程序对象和服务主体对象](../active-directory/active-directory-application-objects.md)



**将代码添加到您的 Web 应用程序要使用的证书**现在我们将添加到您的 Web 应用程序访问证书并将其用于身份验证的代码。 

首先是代码访问证书。 

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint, 
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


请注意，StoreLocation CurrentUser 而不是 LocalMachine。 并且，我们提供 false，查找方法因为我们使用测试证书。


下一步是使用 CertificateHelper，并创建 ClientAssertionCertificate 的需要进行身份验证的代码。 

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


这是新的代码，以获取访问令牌。 这将替换上面的 GetToken 方法。 我赋予它一个不同的名称，为方便起见。 

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

我已将此代码的所有放到 Web 应用程序项目的 Utils 类，以便于使用。 

在 Application_Start 方法中是最后的代码更改。 首先，我们需要调用 GetCert() 方法来加载 ClientAssertionCertificate。 然后我们更改在创建新的 KeyVaultClient 时，我们提供的回调方法。 请注意，此替代我们有上面的代码。 

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**添加到您的 Web 应用程序的证书**将证书添加到您的 Web 应用程序是一个简单的两步过程。 首先，转到 Azure 门户并导航到您的 Web 应用程序。 在为您的 Web 应用程序设置刀片式服务器，请单击"自定义的域和 SSL"条目。 在您打开刀片式服务器将能够上载之上，KVWebApp.pfx 创建的证书，请确保您记得 pfx 为密码。 

![将证书添加到在 Azure 门户 Web 应用程序][2]


您需要做的最后一件事情是将应用程序设置添加到您的 Web 应用程序具有该名称的网站\_负载\_证书，值为 *。 这将确保加载所有的证书。 如果您想要加载您上载的证书，您可以输入其指纹的逗号分隔列表。 

若要了解有关将证书添加到 Web 应用程序的详细信息，请参阅[在 Azure 网站应用程序中使用的证书](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)



## <a id="next"></a>下一步行动 ##


有关编程参考信息，请参阅[Azure 密钥存储库 C# 客户端 API 参考](https://msdn.microsoft.com/library/azure/dn903628.aspx)。


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
 

测试
