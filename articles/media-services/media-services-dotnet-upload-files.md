---
ms.openlocfilehash: 287b67b6594efd1cb1a57c088815fd6f6b2da89d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将文件上载到使用.NET 的介质服务帐户" 
    description="了解如何通过创建并上载资产进入媒体服务媒体内容。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2015" 
    ms.author="juliako"/>



#将文件上载到使用.NET 的介质服务帐户

[AZURE.INCLUDE [media-services-selector-upload-files](../../includes/media-services-selector-upload-files.md)]

媒体服务中您上载 （或接收） 到资产数字文件。 **资产**实体可以包含视频、 音频、 图像、 缩略图集合，文本跟踪和显示字幕文件 （以及有关这些文件的元数据。） 一旦上载文件的则中以便进行进一步的处理和流云安全地存储您的内容。

在资产中的文件被称为**资产文件**。 **AssetFile**实例和实际的媒体文件是两个不同对象。 AssetFile 实例包含有关媒体文件的元数据，而该媒体文件包含实际的媒体内容。

创建资产时，可以指定以下加密选项。 

- 使用**无**-不加密。 这是默认值。 注意，使用此选项时您的内容不受在传输过程中或在存储中存放。
如果您打算提供 MP4 使用渐进式下载，请使用此选项。 
- **CommonEncryption** -使用此选项，如果您要上载已加密和常见的加密或 PlayReady （例如，平滑流式处理保护与 PlayReady DRM） 的 DRM 保护的内容。
- **EnvelopeEncrypted** – 使用此选项，如果您要上载 HLS 使用 AES 加密。 请注意，文件必须具有已编码加密转换管理器。
- **StorageEncrypted** -加密使用本地 AES 256 位加密，然后上载到存储位置的 Azure 存储加密存放您清除内容。 未自动加密和放在编码之前加密的文件系统使用存储加密保护的资产，而且 （可选） 重新加密之前上载回作为一个新的输出资产。 您想要高质量输入的媒体文件安全使用强加密存放在磁盘上时，针对存储加密的主要的用例。

    介质服务提供对您的资产，不转移的线类似数字版权管理器 (DRM) 的磁盘上存储加密。

    如果您的资产存储加密，您必须配置资产交付策略。 有关更多信息，请参见[配置资产交付策略](media-services-dotnet-configure-asset-delivery-policy.md)。

如果为您的资产进行加密使用**CommonEncrypted**选项或**EnvelopeEncypted**选项指定时，需要将您的资产与**ContentKey**相关联。 有关详细信息，请参阅[如何创建 ContentKey](media-services-dotnet-create-contentkey.md)。 

如果您为您的资产进行加密使用**StorageEncrypted**选项指定，.NET 媒体服务 SDK 将创建您的资产**StorateEncrypted** **ContentKey** 。

>[AZURE.NOTE]在流式内容 (例如，http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters。) 生成 Url 时，介质服务使用 IAssetFile.Name 属性的值出于此原因，不允许百分比编码。 **Name**属性的值不能有任何下列[百分比编码保留字符](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): ！ *' （);: @& = + $/ [] ? %#"。 而且，只能有一个 '。 为文件扩展名。

本主题演示如何使用媒体服务.NET SDK 以及媒体服务.NET SDK 扩展来将文件上载到介质服务资产。

 
## 将介质服务.NET sdk 单文件上载 

下面的代码示例使用.NET SDK 来执行以下任务︰ 

- 创建空的资产。
- 创建一个 AssetFile 实例，我们想要与该资产相关联。
- 创建一个 AccessPolicy 实例，对资产定义的权限和访问持续时间。
- 创建定位程序实例，提供对资产的访问。
- 将单个媒体文件上载到介质服务。 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##将多个介质服务.NET sdk 文件上传 

下面的代码演示如何创建资产和上载多个文件。

该代码执行以下任务︰
    
-   创建空资产使用上一步中定义的 CreateEmptyAsset 方法。
    
-   创建一个**AccessPolicy**实例，对资产定义的权限和访问持续时间。
    
-   创建**定位程序**实例，提供对资产的访问。
    
-   创建一个**BlobTransferClient**实例。 此类表示对 Azure blob 进行操作的客户端。 在本例中我们使用客户端的上载进度。 
    
-   枚举指定目录中的文件并创建一个**AssetFile**实例的每个文件。
    
-   会将文件上载到介质服务使用**UploadAsync**方法。 
    
>[AZURE.NOTE] 使用 UploadAsync 方法以确保未阻止调用并上载文件的并行。
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



当上传了大量的资产，请考虑以下。

- 创建一个新的**CloudMediaContext**对象，每个线程。 **CloudMediaContext**类不是线程安全的。
 
- 增加到较高的类似 5 的值，默认值为 2 NumberOfConcurrentTransfers。 设置此属性会影响**CloudMediaContext**的所有实例。 
 
- 保留默认值为 10 ParallelTransferThreadCount。
 
##<a id="ingest_in_bulk"></a>使用介质服务.NET SDK 的批量接收资产 

上传大型资源文件，可以在资产创建过程是一个瓶颈。 接收在批量或"批量接收"的资产，涉及到分离资产创建从上载过程。 若要使用批量接收方法，创建描述资产和及其相关联的文件的清单 (IngestManifest)。 然后使用上载您选择的方法将关联的文件上载到清单的 blob 容器。 Microsoft Azure 媒体服务监视与清单相关联的 blob 容器。 一旦文件将上载到 blob 容器，Microsoft Azure 媒体服务完成资产创建基于清单 (IngestManifestAsset) 中的资产配置。


若要创建新的 IngestManifest 调用公开的 CloudMediaContext 的 IngestManifests 集合的创建方法。 此方法将创建新的 IngestManifest 与您提供的清单名称。

    IIngestManifest manifest = context.IngestManifests.Create(name);

创建将与大容量 IngestManifest 相关联的资产。 配置所需的加密选项卡上的批量接收的资产。

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

IngestManifestAsset 将资产与大容量 IngestManifest 对于批量接收相关联。 它还将构成每项资产 AssetFiles 相关联。 若要创建 IngestManifestAsset，使用服务器上下文创建方法。

下面的示例演示如何添加两个新 IngestManifestAssets 关联到批量以前创建的两个资产接收清单 每个 IngestManifestAsset 还将为每个资产将上载文件的一组关联批量接收过程。  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
您可以使用任何高速客户端应用程序能够将资源文件上载到 blob 存储容器提供的 IngestManifest 的**IIngestManifest.BlobStorageUriForUpload**属性的 URI。 一个值得注意的高速上载服务是[Aspera 点播 Azure 应用程序](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6)。 您还可以编写代码来上载的资产文件，如下面的代码示例中所示。
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

有关使用本主题中的示例将资产文件上载的代码如下面的代码示例所示。
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

您可以确定通过轮询的统计信息属性的**IngestManifest**与**IngestManifest**相关联的所有资产批量都接收的进度。 为了更新进度信息，必须使用新的**CloudMediaContext**每次轮询的统计属性。

下面的示例演示轮询由其**Id**IngestManifest。
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##上载文件使用.NET SDK 扩展 

下面的示例演示如何使用.NET SDK 扩展单文件上载。 在这种情况下使用**CreateFromFile**方法，但异步版本也是可用的 (**CreateFromFileAsync**)。 **CreateFromFile**方法允许您指定要上载的文件的进度报告的文件名称、 加密选项和回调。


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

下面的示例调用 UploadFile 函数，并指定存储加密作为资产创建选项。  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##下一步行动
现在，您已上载到介质服务的资产，转到[如何获取媒体处理器][]主题。

[如何获取媒体处理器]: media-services-get-media-processor.md
 

测试
