---
ms.openlocfilehash: c82ddd0ad662fb69ae67a47080c6827e8b41b3e8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="连接到使用 REST API，媒体服务帐户" 
    description="本主题演示如何连接到媒体服务 uisng REST API。" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/11/2015"
    ms.author="juliako"/>


# 连接到使用 REST API，介质服务的介质服务帐户

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-connect_programmatically.md)
- [REST](media-services-rest-connect_programmatically.md)

本主题介绍如何使用媒体服务 REST API 编程时获得以编程方式连接到 Microsoft Azure 媒体服务。

两件事都需要访问 Microsoft Azure 媒体服务时︰ 由 Azure 访问控制服务 (ACS) 和媒体服务的 URI 本身提供一个访问令牌。 您可以使用创建这些请求，只要您指定正确的标头值并通过访问令牌中正确地调用时到媒体服务时所需的任何手段。

以下步骤描述了最常见的流媒体服务 REST API，用于连接到媒体服务时︰

1. 获取一个访问令牌 
2. 连接到媒体服务 URI 

    >[AZURE.NOTE] 已成功连接到 https://media.windows.net 之后, 您将收到指定另一个媒体服务 URI 的 301 重定向。 必须使新的 URI 对的后续调用。
您还可能收到包含 ODATA API 的元数据描述的 HTTP/1.1 200 响应。

3. 过帐到新的 URL 您随后 API 调用。 

    例如，如果之后试图进行连接，您有以下︰

        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    应发布您对 https://wamsbayclus001rest-hs.cloudapp.net/api/ 的后续 API 调用。

##获取一个访问令牌

直接通过 REST API 访问介质服务，从 ACS 检索访问令牌并成为该服务的每个 HTTP 请求过程中使用它。 此标记是类似于其他基于访问索赔提供的 HTTP 请求和使用 OAuth v2 协议标头中的 ACS 提供的令牌。 在直接连接到媒体服务之前不需要任何其他系统必备组件。

下面的示例显示的 HTTP 请求标头和正文用来检索令牌。

**标题**︰

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**正文**︰

您需要证明此请求; 正文中的 client_id 和 client_secret 值client_id 和 client_secret 为帐户名和 AccountKey 值中，分别对应。 这些值被提供给您通过介质服务，当您设置您的帐户。 

注意，AccountKey 的介质服务帐户必须是 URL 编码 （请参阅[%编码](http://tools.ietf.org/html/rfc3986#section-2.1)时使用它为您的访问令牌请求中的 client_secret 值。

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


例如︰ 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


下面的示例演示包含访问 HTTP 响应令牌在响应正文中。

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
建议以缓存到外部存储的"access_token"和"expires_in"值。 标记数据以后无法从存储中检索并重新在媒体服务 REST API 调用中使用。 这是在令牌可以安全地共享多个进程或计算机之间的方案尤其有用。

确保监视访问令牌的"expires_in"值，并根据需要更新使用新标记的 REST API 调用。

###连接到媒体服务 URI

根为媒体服务的 URI 是 https://media.windows.net/。 最初应连接到此 URI，并且如果在响应中得到 301 重定向，则应该对新的 URI 的后续调用。 此外，不要请求中使用的任何自动重定向/遵循逻辑。 HTTP 谓词和请求正文不能转发到新的 URI。

请注意，根于上传和下载资产文件的 URI https://yourstorageaccount.blob.core.windows.net/ 其中存储帐户名称是相同媒体服务帐户设置过程中使用。

下面的示例演示对媒体服务根 URI (https://media.windows.net/) 的 HTTP 请求。 请求在响应中获取 301 重定向。 后续的请求正在使用新的 URI (https://wamsbayclus001rest-hs.cloudapp.net/api/)。     

**HTTP 请求**︰
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.9
    Accept: application/json
    Host: media.windows.net


**HTTP 响应**︰
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**HTTP 请求**（使用新的 URI）︰
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.9
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP 响应**︰
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] 一旦获得新的 URI，这是应该用于相互通信介质服务的 URI。 


<!-- Anchors. -->


<!-- URLs. -->
 
测试
