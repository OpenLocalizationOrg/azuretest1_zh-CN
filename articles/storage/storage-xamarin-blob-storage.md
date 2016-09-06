---
ms.openlocfilehash: e22d619e7bf88c6c54086ad5f225381014b25e13
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用来自 Xamarin （预览） 的 Blob 存储 |Microsoft Azure" 
    description="Xamarin 预览 Azure 存储客户端库使开发人员能够使用其本机用户界面创建 iOS 和 Android，Windows 应用商店应用程序。 本教程展示如何使用 Xamarin 来创建使用 Azure Blob 存储的 Android 应用程序。" 
    services="storage" 
    documentationCenter="xamarin" 
    authors="tamram" 
    manager="carolz" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2015" 
    ms.author="tamram"/>

# 如何使用 Blob 存储从 Xamarin （预览）

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概述

Xamarin 使开发人员能够使用共享的 C# 代码库来使用其本机用户界面创建 iOS 和 Android，Windows 应用商店应用程序。 Xamarin Azure 存储客户端库是预览库;请注意，可能会在将来更改。

本教程展示如何使用 Azure Blob 存储与 Xamarin Android 应用程序。 若要了解有关 Azure 存储到代码之前，请参阅本文档结尾处的[下一步行动](#next-steps)。

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## 生成一个签名，共享的访问

在 Azure 存储客户端库为 Xamarin，不能身份验证到 Azure 存储帐户使用您的帐户的访问密钥的访问。 这是为了防止被分发到可能下载您的应用程序的用户帐户凭据。 相反，我们建议使用共享的访问签名 (SA) 不会公开您的帐户凭据。

在本指南中，我们将使用 Azure PowerShell 以生成 SAS 令牌。 然后，我们将创建一个使用生成的 SAS 的 Xamarin 应用程序。

首先，您需要安装 Azure PowerShell。 了解[如何安装和配置 Azure PowerShell](../powershell-install-configure.md#Install)有关的说明。

接下来，打开 Azure PowerShell 并运行以下命令。 请记住，替换`ACCOUNT_NAME`，`ACCOUNT_KEY== `与您的存储帐户凭据。 更换`CONTAINER_NAME`与您选择的名称。

    PS C:\> $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
    PS C:\> New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
    PS C:\> $now = Get-Date
    PS C:\> New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

新的容器的共享的访问签名 URI 应类似如下︰

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

第二天，在容器创建的共享的访问签名均有效。 签名的 blob 容器内授予完全权限 （*如*读取、 写入、 删除和列表）。

关于共享的访问签名的详细信息，请参见[.NET 的 SAS 教程](storage-dotnet-shared-access-signature-part-2.md)。

## 创建一个新的 Xamarin 应用程序

对于本教程，我们将创建我们的 Xamarin 应用程序在 Visual Studio 中。 请按照下列步骤以创建应用程序︰

1. 下载并安装[Visual Studio](https://www.visualstudio.com/)。
2. 下载并安装[Xamarin](http://xamarin.com/platform)。
3. 打开 Visual Studio，并选择**文件 > 新建 > 项目 > Android > 空白 App(Android)**。
4. 用鼠标右键单击解决方案资源管理器窗格中的项目，然后选择**管理 NuGet 程序包**。 然后**Azure 存储**中搜索并安装**Azure 存储 4.4.0-preview**。

现在应该有一个应用程序，您可以单击一个按钮，自动增加的计数器。

## 使用共享的访问签名来执行容器操作

接下来，添加代码以执行一系列容器操作使用 SAS 生成的 URI。

首先将添加下面的**using**语句︰

    using System.IO;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.Storage.Blob;


接下来，添加 SAS 标记的行。 更换`"SAS_URI"`在 Azure PowerShell 生成 SAS URI 的字符串。 然后将添加的行调用`UseContainerSAS`，我们将创建下面的方法。 请注意，已委托之前添加的**async**关键字。


    public class MainActivity : Activity
    {
        int count = 1;
        string sas = "SAS_URI";
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.Main);

            // Get our button from the layout resource, and attach an event to it
            Button button = FindViewById<Button>(Resource.Id.MyButton);

            button.Click += async delegate  {
                button.Text = string.Format("{0} clicks!", count++);
                await UseContainerSAS(sas);
            };
     }

添加新的方法， `UseContainerSAS`，在`OnCreate`方法。

    static async Task UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));
        string date = DateTime.Now.ToString();
        try
        {
            //Write operation: write a new blob to the container.
            CloudBlockBlob blob = container.GetBlockBlobReference("sasblob_" + date + ".txt");

            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new
            MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                await blob.UploadFromStreamAsync(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (Exception e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        try
        {
            //Read operation: Get a reference to one of the blobs in the container and read it.
            CloudBlockBlob blob = container.GetBlockBlobReference("sasblob_” + date + “.txt");
            string data = await blob.DownloadTextAsync();

            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine("Blob contents: " + data);
        }
        catch (Exception e)
        {
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine();
        }
        Console.WriteLine();
        try
        {
            //Delete operation: Delete a blob in the container.
            CloudBlockBlob blob = container.GetBlockBlobReference("sasblob_” + date + “.txt");
            await blob.DeleteAsync();

            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (Exception e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
    }

## 运行应用程序

您现在可以在 Android 设备或仿真程序中运行此应用程序。

虽然此入门教程侧重于 Android，您可以使用`UseContainerSAS`在 iOS 或 Windows 应用商店应用程序中的方法。 Xamarin 还允许开发人员创建 Windows Phone 应用程序;但是，我们的库还不支持此。

## 下一步行动

在本教程中，您学习了如何使用 Xamarin 应用程序中使用 Azure Blob 存储和 SAS。 作为进一步的练习，可以应用类似的模式来生成的 Azure 表或队列的 SAS 标记。

通过签出下面的链接了解有关 blob、 表和队列的详细信息︰

[介绍 Microsoft Azure 存储](storage-introduction.md)  
[如何使用.NET 中的 Blob 存储](storage-dotnet-how-to-use-blobs.md)  
[如何使用.NET 中的表存储](storage-dotnet-how-to-use-tables.md)  
[如何使用队列存储从.NET](storage-dotnet-how-to-use-queues.md)
 