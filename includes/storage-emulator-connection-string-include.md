---
ms.openlocfilehash: 7a3465a726c0055b31713597f31574f9b36e8816
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
共享密钥身份验证存储仿真器支持单个固定的帐户和已知的身份验证密钥。 此帐户和密钥是用于存储仿真器允许的唯一共享密钥凭据。 它们是︰

    Account name: devstoreaccount1
    Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
    
> [AZURE.NOTE] 身份验证密钥存储模拟器支持仅供测试的客户端身份验证代码功能。 不，它不提供任何安全目的。 不能使用您的生产存储帐户和密钥存储仿真器。 此外请注意，不应与生产数据使用开发帐户。
>
> 请注意，存储仿真器支持仅通过 HTTP 连接。 但是，HTTPS 是用于访问资源生产 Azure 存储帐户中的推荐的协议。
 
#### 连接到仿真程序帐户使用快捷方式

从您的应用程序连接到存储仿真程序的最简单方法是配置快捷方式引用的应用程序的配置文件中的连接字符串`UseDevelopmentStorage=true`。 以下是存储仿真器在 app.config 文件中的连接字符串示例︰ 

    <appSettings>
      <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
    </appSettings>

#### 连接到仿真程序帐户使用众所周知的帐户名称和密钥

若要创建引用的仿真程序的帐户名称和密钥的连接字符串，请注意，必须指定您希望从仿真程序的连接字符串中使用的服务的每个端点。 这是必需的因此，连接字符串将引用不同于生产存储帐户的仿真程序终结点。 例如，您的连接字符串的值将如下所示︰

    DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
    AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
    BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
    TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
    QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1; 

此值等同于上面所示的快捷方式`UseDevelopmentStorage=true`。

#### 指定 HTTP 代理服务器

您还可以指定您要测试存储仿真程序对您的服务时要使用的 HTTP 代理。 这可用于调试的存储服务操作时观察 HTTP 请求和响应。 若要指定代理服务器，请添加`DevelopmentStorageProxyUri`选项设置为连接字符串，并将其值设置到代理的 URI。 例如，下面是指向存储仿真程序的配置 HTTP 代理服务器的连接字符串︰

    UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
