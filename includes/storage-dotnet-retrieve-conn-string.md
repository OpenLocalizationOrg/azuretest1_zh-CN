---
ms.openlocfilehash: a9bd1d58a21d5ed32f207be05212e702070d644b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
### 检索连接字符串
您可以使用**CloudStorageAccount**类型来表示您的存储帐户信息。 如果您正在使用 Azure 项目模板和/或具有到 Microsoft.WindowsAzure.CloudConfigurationManager 命名空间的引用，您可以使用**CloudConfigurationManager**类型以检索您的存储连接字符串和存储帐户信息从 Azure 服务配置︰

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

如果您为应用程序而不参考 Microsoft.WindowsAzure.CloudConfigurationManager，并且您的连接字符串位于`web.config`或`app.config`如上述所示，然后您可以使用**ConfigurationManager**来检索连接字符串。  您需要向项目中添加对 System.Configuration.dll 的引用并添加它的另一个命名空间声明︰

    using System.Configuration;
    ...
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString);
