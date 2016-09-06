---
ms.openlocfilehash: aa78d212d46b62ad9f4406d5702658b982de99ef
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="对资产使用媒体编码标准进行编码的方式" 
    description="本主题演示如何使用.NET 编码使用媒体编码 Strandard 的资产。" 
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
    ms.date="08/24/2015"  
    ms.author="juliako"/>


#对资产使用媒体编码标准进行编码的方式

编码作业是一种媒体服务中最常见的处理操作。 创建从到另一种编码转换媒体文件的编码工作。 在编码时，您可以使用介质服务内置 Media 编码器。 您还可以使用编码器提供的媒体服务合作伙伴;第三方编码器能够通过 Azure 市场。 

本主题演示如何使用.NET 来对您的资产与媒体编码标准进行编码。 媒体编码标准被配置使用的编码器预设所描述[这里](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409)之一。

>[AZURE.NOTE]媒体处理器的当前版本要求您在编码预设为整个 XML 或 JSON 字符串中传递。 愿意支持命名字符串中传递的"H264 多比特率 720 p"，将会很快通过服务更新可用。

建议始终对夹层文件到 MP4 设置，然后将组转换为所需的格式，使用[动态打包](media-services-dynamic-packaging-overview.md)自适应比特率进行编码。 利用动态打包，必须先从中计划交付给您的内容的流终结点获取至少一个点播流计价单位。 有关详细信息，请参阅[如何规模的媒体服务](media-services-manage-origins.md#scale_streaming_endpoints)。

如果输出资产存储加密，您必须配置资产交付策略。 有关更多信息，请参见[配置资产交付策略](media-services-dotnet-configure-asset-delivery-policy.md)。

##示例

下面的代码示例使用介质服务.NET SDK 来执行以下任务︰

- 创建编码的作业。
- 获取对媒体编码标准编码器的引用。
- 加载预设的 XML 从一种预设显示[在此处](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409)。
- 向作业添加单个编码任务。 
- 指定要编码输入的资产。
- 创建输出资产将包含已编码的资产。
- 添加事件处理程序以检查作业进度。
- 提交作业。
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset, string pathToLocalPresetFile)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
            // Load the XML (or JSON) from the local file
            string configuration = File.ReadAllText(pathToLocalPresetFile);
        
            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                processor,
                configuration,
                TaskOptions.None);
        
            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);
        
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

##请参见 

[媒体服务编码概述](media-services-encode-asset.md)测试
