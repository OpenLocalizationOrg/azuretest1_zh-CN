---
ms.openlocfilehash: 6ce50565ab3090949e29c6aab2a96082cd464ffb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何对资产使用 Azure Media 编码器进行编码" 
    description="了解如何使用 Azure Media 编码器编码在媒体服务上的媒体内容。 代码示例编写的 C# 和用于.NET 的媒体服务 SDK。" 
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
    ms.date="08/24/2015" 
    ms.author="juliako"/>


#如何对资产使用 Azure Media 编码器进行编码


> [AZURE.SELECTOR]
- [REST](media-services-rest-encode-asset.md)
- [.NET](media-services-dotnet-encode-asset.md)
- [门户](media-services-manage-content.md#encode)

##概述

为了通过互联网提供数字视频必须压缩介质。 数字视频文件非常大，可能会太大，无法提供通过互联网或您客户的设备才能正确显示。 编码是压缩视频和音频，因此您的客户可以查看您的媒体的过程。

编码作业是一种媒体服务中最常见的处理操作。 创建从到另一种编码转换媒体文件的编码工作。 在编码时，您可以使用介质服务内置 Media 编码器。 您还可以使用编码器提供的媒体服务合作伙伴;第三方编码器能够通过 Azure 市场。 您可以指定编码任务通过使用预设的字符串定义为您的编码器，或通过使用预设的配置文件的详细的信息。 若要查看可用的预设的类型，请参阅[任务预设 Azure 媒体服务](https://msdn.microsoft.com/library/azure/dn619392.aspx)。 如果您使用第三方编码，您应[验证您的文件](https://msdn.microsoft.com/library/azure/dn750842.aspx)。

建议始终对夹层文件到 MP4 设置，然后将组转换为所需的格式，使用[动态打包](media-services-dynamic-packaging-overview.md)自适应比特率进行编码。 利用动态打包，必须先从中计划交付给您的内容的流终结点获取至少一个点播流计价单位。 有关详细信息，请参阅[如何规模的媒体服务](media-services-manage-origins.md#scale_streaming_endpoints)。

如果输出资产存储加密，您必须配置资产交付策略。 有关更多信息，请参见[配置资产交付策略](media-services-dotnet-configure-asset-delivery-policy.md)。

##与单个编码任务创建作业 

当与 Azure Media 编码器编码，您可以使用指定任务配置预置[此处](https://msdn.microsoft.com/library/azure/dn619389.aspx)。

###使用介质服务 SDK.NET  

下面的**EncodeToAdaptiveBitrateMP4Set**方法创建编码作业，并向作业添加单个编码任务。 任务使用"Azure Media 编码器"编码为"H264 自适应比特率 MP4 集 720 p"。 

    static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
    {
        var encodingPreset = "H264 Adaptive Bitrate MP4 Set 720p";

        IJob job = _context.Jobs.Create(String.Format("Encoding {0} into to {1}",
                                inputAsset.Name,
                                encodingPreset));

        var mediaProcessors = GetLatestMediaProcessorByName("Azure Media Encoder");

        ITask encodeTask = job.Tasks.AddNew("Encoding", mediaProcessors, encodingPreset, TaskOptions.None);
        
        encodeTask.InputAssets.Add(inputAsset);

        // Specify the storage-encrypted output asset.
        encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), 
            AssetCreationOptions.StorageEncrypted);


        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
        job.Submit();
        job.GetExecutionProgressTask(CancellationToken.None).Wait();

        return job.OutputMediaAssets[0];
    }

    private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
    {
        Console.WriteLine("Job state changed event:");
        Console.WriteLine("  Previous state: " + e.PreviousState);
        Console.WriteLine("  Current state: " + e.CurrentState);
        switch (e.CurrentState)
        {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
                break;
            case JobState.Canceling:
            case JobState.Queued:
            case JobState.Scheduled:
            case JobState.Processing:
                Console.WriteLine("Please wait...\n");
                break;
            case JobState.Canceled:
            case JobState.Error:

                // Cast sender as a job.
                IJob job = (IJob)sender;

                // Display or log error details as needed.
                break;
            default:
                break;
        }
    }

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
           ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }

###使用介质服务 SDK.NET 扩展

    static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
    {
        // 1. Prepare a job with a single task to transcode the specified mezzanine asset
        //    into a multi-bitrate asset.
        IJob job = _context.Jobs.CreateWithSingleTask(
            MediaProcessorNames.AzureMediaEncoder,
            MediaEncoderTaskPresetStrings.H264AdaptiveBitrateMP4Set720p,
            asset,
            "Adaptive Bitrate MP4",
            AssetCreationOptions.None);

        Console.WriteLine("Submitting transcoding job...");

        // 2. Submit the job and wait until it is completed.
        job.Submit();
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;

        Console.WriteLine("Transcoding job finished.");

        IAsset outputAsset = job.OutputMediaAssets[0];

        return outputAsset;
    } 

##链接任务创建作业 

在很多应用程序方案中，开发人员要创建一系列的处理任务。 在介质服务，您可以创建一系列链接的任务。 每个任务执行不同的处理步骤，可以使用不同的媒体处理器。 链接的任务可以递交资产从一个任务到另一个，在资产上执行任务的线性序列。 但是，在作业中执行的任务不是需要序列中。 创建链接的任务时，在单个**IJob**对象创建链接的**ITask**对象。

>[AZURE.NOTE] 目前有 30 个任务每个作业任务的限制。 如果需要链接 30 多个任务，请创建包含任务的多个作业。

下面的**CreateChainedTaskEncodingJob**方法创建一个包含两个链接的任务的作业。 因此，该方法将返回包含两个输出资产的作业。

    
    public static IJob CreateChainedTaskEncodingJob(IAsset asset)
    {
        // Declare a new job.
        IJob job = _context.Jobs.Create("My task-chained encoding job");

        // Set up the first task to encode the input file.

        // Get a media processor reference
        IMediaProcessor processor = GetLatestMediaProcessorByName("Azure Media Encoder");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My encoding task",
            processor,
           "H264 Adaptive Bitrate MP4 Set 720p",
            TaskOptions.ProtectedConfiguration);

        // Specify the input asset to be encoded.
        task.InputAssets.Add(asset);

        // Specify the storage-encrypted output asset.
        task.OutputAssets.AddNew("My storage-encrypted output asset",
            AssetCreationOptions.StorageEncrypted);

        // Set up the second task to decrypt the encoded output file from 
        // the first task.

        // Get another media processor instance
        IMediaProcessor decryptProcessor = GetLatestMediaProcessorByName("Storage Decryption");

        // Declare the decryption task. 
        ITask decryptTask = job.Tasks.AddNew("My decryption task",
            decryptProcessor,
            string.Empty,
            TaskOptions.None);

        // Specify the input asset to be decrypted. This is the output 
        // asset from the first task. 
        decryptTask.InputAssets.Add(task.OutputAssets[0]);

        // Specify an output asset to contain the results of the job. 
        // This should have AssetCreationOptions.None. 
        decryptTask.OutputAssets.AddNew("My decrypted output asset",
            AssetCreationOptions.None);

        // Use the following event handler to check job progress. 
        job.StateChanged += new
            EventHandler<JobStateChangedEventArgs>(JobStateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish. 
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        //return job that contains two output assets.
        return job;
    }


##请参见 

[媒体服务编码概述](media-services-encode-asset.md)

 
测试
