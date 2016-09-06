---
ms.openlocfilehash: 6fcfb3c425e6e34397e1cd2cdee6eda967e44631
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在滚动存储访问键之后更新介质服务" 
    description="该文章给如何滚动存储访问键后更新媒体服务指导。" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015"
    ms.author="juliako"/>

#如何︰ 更新介质服务后滚动存储访问键

创建一个新的 Azure 媒体服务帐户时，您还需要选择 Azure 存储帐户，用来存储您的媒体内容。 请注意，您可以添加多个存储帐户到介质服务帐户。

当创建新的存储帐户时，Azure 将生成两个 512 位存储访问键，用来验证访问您的存储帐户。 为了保护您存储的连接更安全，建议以定期重新生成并旋转您存储的访问密钥。 为了使您能够维护存储帐户时重新生成的其他访问键使用一个访问键的连接提供了两个 （主和次） 的访问键。 此过程也称为"滚动访问键"。

介质服务 （主要或辅助） 存储键之一上具有依赖项。 具体而言，用于流或下载您的资产的定位器取决于访问键。 当滚动存储访问键，您还需要确保因此更新您定位器将流式服务没有中断。

>[AZURE.NOTE]重新生成存储密钥后，必须确保与介质服务同步更新。 

本主题描述步骤的步骤来部署存储键和更新介质服务使用适当的存储密钥。 请注意，是否您有多个存储帐户，您将执行此过程与每个存储帐户。

>[AZURE.NOTE]执行生产帐户上本主题中介绍的步骤之前, 一定要测试它们在生产前帐户上。


## 步骤 1︰ 重新生成辅助存储访问键

启动重新生成辅助存储密钥。 默认情况下，辅助键不通过介质服务使用。  如何准备存储密钥的信息，请参阅[如何︰ 查看、 复制和访问密钥的重新生成存储](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)。
  
##<a id="step2"></a>步骤 2︰ 更新介质服务使用新的辅助存储密钥

更新的介质服务使用辅助存储的访问键。 可以使用以下两种方法之一将重新生成的存储密钥与介质服务同步。

- 使用 Azure 门户︰ 选择介质服务帐户，然后单击的门户窗口底部的"管理密钥"图标。 根据所需的介质服务与同步到哪个存储键，选择同步主键或同步辅助键按钮。 在这种情况下，使用辅助键。

- 使用介质服务管理 REST API。 

    下面的代码示例演示如何构造 https://endpoint/<subscriptionId>/服务/mediaservices/帐户/<accountName>/StorageAccounts/<storageAccountName>/Key 请求以便与介质服务同步指定的存储密钥。 在这种情况下，使用辅助存储键值。 有关详细信息，请参阅[如何︰ 使用介质服务管理 REST API，](http://msdn.microsoft.com/en-us/library/azure/dn167656.aspx)。
 
        public void UpdateMediaServicesWithStorageAccountKey(string mediaServicesAccount, string storageAccountName, string storageAccountKey)
        {
            var clientCert = GetCertificate(CertThumbprint);
        
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(string.Format("{0}/{1}/services/mediaservices/Accounts/{2}/StorageAccounts/{3}/Key",
                Endpoint, SubscriptionId, mediaServicesAccount, storageAccountName));
            request.Method = "PUT";
            request.ContentType = "application/json; charset=utf-8";
            request.Headers.Add("x-ms-version", "2011-10-01");
            request.Headers.Add("Accept-Encoding: gzip, deflate");
            request.ClientCertificates.Add(clientCert);
        
        
            using (var streamWriter = new StreamWriter(request.GetRequestStream()))
            {
                streamWriter.Write("\"");
                streamWriter.Write(storageAccountKey);
                streamWriter.Write("\"");
                streamWriter.Flush();
            }
        
            using (var response = (HttpWebResponse)request.GetResponse())
            {
                string jsonResponse;
                Stream receiveStream = response.GetResponseStream();
                Encoding encode = Encoding.GetEncoding("utf-8");
                if (receiveStream != null)
                {
                    var readStream = new StreamReader(receiveStream, encode);
                    jsonResponse = readStream.ReadToEnd();
                }
            }
        }

之后此现有定位器 （即旧存储密钥具有依赖项） 的更新。

>[AZURE.NOTE]等待 30 分钟之前执行任何操作与介质服务 （例如，创建新定位器） 以挂起的作业上防止产生任何影响。

##步骤 3︰ 更新定位器 

30 分钟后可以更新现有定位器，使他们所参与的新的辅助存储键依赖项。  

若要更新定位符上的到期日期，请使用[其余部分](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator )或[.NET](http://go.microsoft.com/fwlink/?LinkID=533259) Api。 注意当您更新 SAS 定位器的到期日期，该 URL 将更改。 

##步骤 5︰ 重新生成主存储访问键

重新生成主存储访问键。 如何准备存储密钥的信息，请参阅[如何︰ 查看、 复制和访问密钥的重新生成存储](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)。

##第 6 步︰ 更新介质服务使用新的主存储密钥
    
使用相同的过程中所述[步骤 2](media-services-roll-storage-access-keys.md#step2)只这一次新的主存储访问键与同步媒体服务帐户。

>[AZURE.NOTE]等待 30 分钟之前执行任何操作与介质服务 （例如，创建新定位器） 以挂起的作业上防止产生任何影响。

##第 7 步︰ 更新定位器  

30 分钟后可以更新现有定位器，使他们所参与的新的主存储键依赖项。  

若要更新定位符上的到期日期，请使用[其余部分](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator )或[.NET](http://go.microsoft.com/fwlink/?LinkID=533259) Api。 注意当您更新 SAS 定位器的到期日期，该 URL 将更改。 

 
 
测试
