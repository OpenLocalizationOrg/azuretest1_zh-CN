---
ms.openlocfilehash: 4a46f141678ff3f8e3deb779a63a790fd0087e08
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Windows 应用商店应用程序中使用 Azure 存储 |Microsoft Azure"
    description="了解如何使用 Azure blob、 队列和表来存储数据的 Windows 应用商店应用程序。"
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="adinah" />
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/18/2015"
    ms.author="tamram"/>
# 如何在 Windows 应用商店应用程序中使用 Azure 存储

## 概述

本指南介绍了如何开发利用 Azure 存储 Windows 应用商店应用程序开始。

## 下载所需的工具

- [Visual Studio 2012](http://msdn.microsoft.com/library/windows/apps/br211384)容易构建、 调试、 本地化，包，以及部署 Windows 应用商店应用程序。
- [Azure 存储客户端库的 Windows 运行时](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/11/05/windows-azure-storage-client-library-for-windows-runtime.aspx)提供类库使用 Azure 存储。
- [WCF 数据服务工具为 Windows 应用商店应用程序](http://www.microsoft.com/download/details.aspx?id=30714)扩展 Windows 应用商店应用程序在 Visual Studio 2012 及更高版本的客户端的 OData 支持添加服务引用的经验。

## 开发应用程序

### 做好准备

创建新的 Windows 应用商店应用程序项目，在 Visual Studio 2012 或更高版本︰

![存储-应用程序-存储-与-项目][store-apps-storage-vs-project]

接下来，通过右键单击**引用**，单击**添加引用**，然后浏览到存储客户端库的 Windows 运行您下载的添加对 Azure 存储客户端库的引用︰

![存储应用程序的存储-选择-库][store-apps-storage-choose-library]

### 使用斑点和队列服务库

此时，您的应用程序就可以调用的 Azure Blob 和队列服务。 添加下面的**using**语句，这样可以直接引用 Azure 存储类型︰

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

接下来，添加一个按钮到您的页。 将以下代码添加到其**Click**事件并使用[async 关键字](http://msdn.microsoft.com/library/vstudio/hh156513.aspx)来修改您的事件处理程序方法︰

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

此代码假定您有两个字符串变量、*帐户名*和*accountKey*。 它们代表了您的存储帐户和帐户密钥与该帐户关联的名称。

生成并运行该应用程序。 单击按钮将检查您的帐户中是否存在名为*container1*容器，如果不能创建它。

### 使用表服务库

用于与 Azure 表服务进行通信的类型取决于 WCF 数据服务为 Windows 应用商店应用程序库。 接下来，使用程序包管理器控制台中添加所需的 WCF 库的引用︰

![-应用程序-存储-包的店][store-apps-storage-package-manager]

使用您计算机上的位置点程序包管理器到下面的命令︰

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

此命令将自动添加到您的项目的全部所需的参照。 如果不想使用程序包管理器控制台，可以在本地计算机上的软件包源列表中添加 WCF 数据服务 NuGet 文件夹并将用户界面，通过引用[管理 NuGet 程序包使用对话框](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)中所述。

在引用 WCF 数据服务 NuGet 程序包时更改按钮的**Click**事件中的代码︰

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

此代码将检查名为*表 1*的表是否存在于您的帐户，以及如果不创建它。

您还可以添加参考 Microsoft.WindowsAzure.Storage.Table.dll，下载同一个包中。 此库包含其他功能，如基于反射的序列化和一般的查询。 请注意，此库不支持 JavaScript。



[存储-应用程序-存储-与-项目]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[存储应用程序的存储-选择-库]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[-应用程序-存储-包的店]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
