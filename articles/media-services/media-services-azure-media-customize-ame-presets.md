---
ms.openlocfilehash: f8e80c50942e9c6696ed19627f298c89b6a5a9f5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="通过自定义预设任务操作编码任务" 
    description="Azure 媒体服务编码器允许您将自定义的音频预设值的文件传递到 Azure 媒体编码器。 本主题演示如何自定义预设的文件，为实现下列任务︰ 叠加到现有的视频图像，控制编码器产生的输出文件同名，缝视频。 " 
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
    ms.date="08/11/2015"
    ms.author="juliako"/>

#通过自定义预设任务操作编码任务 

Azure 媒体服务编码器允许您将自定义的音频预设值的文件传递到 Azure 媒体编码器。 本主题介绍如何自定义预设的文件，为实现下列任务︰ 

- 覆盖现有的视频中，将图像 
- 控制编码器产生，输出文件的名称 
- 拼接视频
- 对演示文稿与主要语音进行编码。

##控制 Azure 媒体编码器输出文件的名称 

默认情况下，Azure Media 编码器通过组合输入的资产和编码过程的各种不同属性创建输出文件名。 每个属性标识，如下所述使用宏。

以下是宏的位置命名输出文件的完整列表︰ 以 kbps 为单位指定音频比特率的用来编码音频比特率

- 音频编解码器用于编码的音频编解码器，有效值为︰ AAC、 WMA 和 DDP
- 通道数编码的音频频道数，有效值为︰ 1、 2 或 6
- 默认扩展名 – 默认文件扩展名 
- 语言 BCP 47 表示音频中所用语言的语言代码。 这当前默认为"und"。 
- 原始文件名到 Azure 存储上载的文件的名称
- StreamId – 流 ID 定义的 streamID 特性的<StreamInfo>预设文件中的元素 
- 视频编解码器编码的编码解码器，有效值为︰ H264 和 VC1
- 视频比特率以 kbps 为单位指定的比特率编码视频时使用

在任意排列来控制媒体服务编码器生成的文件的名称，可以组合这些宏。 例如，默认的命名约定为︰

    {Original File Name}_{Video Codec}{Video Bitrate}{Audio Codec}{Language}{Channel Count}{Audio Bitrate}.{Default Extension}

使用[预设](https://msdn.microsoft.com/library/azure/dn554334.aspx)的元素的 DefaultMediaOutputFileName 属性指定文件命名约定。 例如︰

    <Preset DefaultMediaOutputFileName="{Original file name}{StreamId}_LongOutputFileName{Bit Rate}{Video Codec}{Video Bitrate}{Audio Codec}{Audio Bitrate}{Language}{Channel Count}.{Default extension}"
      Version="5.0">
    <MediaFile …>
       <OutputFormat>
          <MP4OutputFormat StreamCompatibility="Standard">
             <VideoProfile>
                <MainH264VideoProfile … >
                   <Streams  AutoSize="False"
                             FreezeSort="False">
                      <StreamInfo StreamId="1"
                                  Size="1280, 720">
                         <Bitrate>
                           <ConstantBitrate Bitrate="3400"
                                            IsTwoPass="False"
                                            BufferWindow="00:00:05" />
                         </Bitrate>
                       </StreamInfo>
                      </Streams>
                   </MainH264VideoProfile>
                </VideoProfile>
             </MP4OutputFormat>
       </OutputFormat>
    </MediaFile>

编码器将插入下划线之间每个宏，例如，上面的配置会导致文件的名称，如︰ MyVideo_H264_4500kpbs_AAC_und_ch2_128kbps.mp4。


##创建叠加

Azure 媒体服务编码器可以叠加到现有视频的 jpg，bmp，gif （tif） 的图像、 视频或音频轨道 （*.wma， *.mp3，*.wav）。 此功能是类似于的表达式编码器 4 (Service Pack 2)。

###与媒体服务编码器的叠加

您可以指定当将出现叠加，呈现，将覆盖的持续时间和图像/视频贴在屏幕叠加显示的位置。 您也可以覆盖淡入和/或淡出。 要覆盖的音频/视频文件可以包含在多个资源或单个资产。 覆盖控制传递到编码器的预设 XML。 预设的架构的完整说明，请参阅 Azure 媒体编码架构。 叠加中指定<MediaFile>元素下面的预设代码段所示︰

    <MediaFile
        ...
        OverlayFileName="%1%"
        OverlayRect="257, 144, 255, 144"
        OverlayOpacity="0.9"
        OverlayFadeInDuration="00:00:02"
        OverlayFadeOutDuration="00:00:02"
        OverlayLayoutMode="Custom"
        OverlayStartTime="00:00:05"
        OverlayEndTime="00:00:10.2120000"
    
        AudioOverlayFileName="%2%"
        AudioOverlayLoop="True"
        AudioOverlayLoopingGap="00:00:00"
        AudioOverlayLayoutMode="WholeSequence"
        AudioOverlayGainLevel="2.2"
        AudioOverlayFadeInDuration="00:00:00"
        AudioOverlayFadeOutDuration="00:00:00">
        ...
    </MediaFile>

###预设的视频或图像叠加

从单个或多个资产可以叠加。 在创建使用多个资产的视频叠加，覆盖文件名指定在 OverlayFileName 属性中使用 %n%语法，其中 n 编码任务输入资产的从零开始的索引。 当使用单个资产创建视频叠加，预设了以下代码段所示直接插入 OverlayFileName 属性中，指定覆盖文件名称︰

示例 1:

    <!-- Multiple Assets -->
    <MediaFile
        ...
        OverlayFileName="%1%"
        OverlayRect="257, 144, 255, 144"
        OverlayOpacity="0.9"
        OverlayFadeInDuration="00:00:02"
        OverlayFadeOutDuration="00:00:02"
        OverlayLayoutMode="Custom"
        OverlayStartTime="00:00:05"
        OverlayEndTime="00:00:10.2120000">

示例 2:

    <!-- Single Asset -->
    <MediaFile
        ...
        OverlayFileName="videoOverlay.mp4"
        OverlayRect="257, 144, 255, 144"
        OverlayOpacity="0.9"
        OverlayFadeInDuration="00:00:02"
        OverlayFadeOutDuration="00:00:02"
        OverlayLayoutMode="Custom"
        OverlayStartTime="00:00:05"
        OverlayEndTime="00:00:10.2120000">

由 OverlayRect 属性控制的位置和大小的视频覆盖。 是会叠加的内容将是重新调整大小以适合此矩形。 由 OverlayOpacity 属性控制不透明度。 有效的值为 0.0-1.0，其中 1.0 是 100%不透明。 叠加将显示在 OverlayStartTime 属性中指定的时间，将由 OverlayEndTime 属性指定的时间结束。 OverlayStartTime 和 OverlayEndTime 应介于内源视频的时间轴。 有关每个覆盖特定属性的详细信息，请参阅 Azure 媒体编码架构。

###对于音频覆盖预设

音频覆盖可以是任何受支持的音频文件格式，例如。 有关受支持的音频文件格式的完整列表，请参阅通过介质服务编码器支持的格式。 此外可以指定音频贴在<MediaFile>元素下面的预设代码段所示︰

    <MediaFile
        ...
        AudioOverlayFileName="%1%" <!-- or AudioOverlayFileName=”audioOverlay.mp3” for overlays from a single asset -->
        AudioOverlayLoop="True"
        AudioOverlayLoopingGap="00:00:00"
        AudioOverlayLayoutMode="Custom"
        AudioOverlayStartTime="00:05:00"
        AudioOverlayEndTime="00:10:00"
        AudioOverlayGainLevel="2.2"
        AudioOverlayFadeInDuration="00:00:00"
        AudioOverlayFadeOutDuration="00:00:00">

对于音频覆盖存储在多个资源，在 AudioOverlayFileName 属性中使用 %n%语法，其中 n 输入资产的编码任务集合的从零开始的索引指定音频覆盖文件名。 对于存储在单个资产的音频覆盖覆盖文件名直接在 AudioOverlayFileName 属性中指定。 AudioOverlayLayoutMode 确定时将会显示音频覆盖。 当设置为"WholeSequence"音频轨道会显示视频的整个期间。 如果设置为"自定义"AudioOverlayStartTime 和 AudioOverlayEndTime 属性确定音频覆盖何时开始和结束。 OverlayStartTime 和 OverlayEndTime 应介于内源视频的时间轴。 所有音频覆盖属性的详细信息，请参阅 Azure 媒体编码架构。 音频覆盖可以结合视频叠加，下面的预设代码段所示︰
    
    <MediaFile
        DeinterlaceMode="AutoPixelAdaptive"
        ResizeQuality="Super"
        AudioGainLevel="1"
        VideoResizeMode="Stretch"
        OverlayFileName="%1%"
        OverlayRect="257, 144, 255, 144"
        OverlayOpacity="0.9"
        OverlayFadeInDuration="00:00:02"
        OverlayFadeOutDuration="00:00:02"
        OverlayLayoutMode="Custom"
        OverlayStartTime="00:00:05"
        OverlayEndTime="00:00:10.2120000"
        AudioOverlayFileName="%2%"
        AudioOverlayLoop="True"
        AudioOverlayLoopingGap="00:00:00"
        AudioOverlayLayoutMode="Custom"
        AudioOverlayStartTime="00:05:00"
        AudioOverlayEndTime="00:10:00"
        AudioOverlayGainLevel="2.2"
        AudioOverlayFadeInDuration="00:00:00"
        AudioOverlayFadeOutDuration="00:00:00">


###提交具有重叠的任务

一旦您已创建的预设的文件，您必须执行以下操作︰

- 上载您的资产
- 加载预设的配置 (注意︰ 下面的代码假定上面的预设。
- 创建作业
- 获取引用媒体服务编码器
- 创建一个任务指定的输入的资产、 预设的配置、 media 编码器中和输出资产的集合
- 提交作业

下面的代码段说明如何执行这些步骤︰

    static public void CreateOverlayJob()
    {
        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(
                        MediaServicesAccountName,
                        MediaServicesAccountKey);
        // Used the cached credentials to create CloudMediaContext.
        _context = new CloudMediaContext(_cachedCredentials);


        // Upload assets to overlay
        IAsset inputAsset1 = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, video1.mp4); // this is the input mezzanine
        IAsset inputAsset2 = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, video2.wmv);// this will be used as a video overlay
        IAsset inputAsset3 = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, video3.wmv); // this will be used as an audio overlay
        
        // Load the preset configuration
        string presetFileName = "OverlayPreset.xml";
        string configuration = File.ReadAllText(presetFileName);
        
        // Create a job
        IJob job = _context.Jobs.Create("A AME overlay job, using " + presetFileName);
                 
        // Get a reference to the media services encoder   
        IMediaProcessor processor = GetLatestMediaProcessorByName("Azure Media Encoder");
            
        // Create a task    
        ITask task = job.Tasks.AddNew("Encode Task for overlay, using " + presetFileName, processor, configuration, TaskOptions.None);
        
        // Add the input assets
        task.InputAssets.Add(inputAsset1);
        task.InputAssets.Add(inputAsset2);
        task.InputAssets.Add(inputAsset3);
        
        // Add the output asset
        task.OutputAssets.AddNew("Result of an overlay job, using " + presetFileName, AssetCreationOptions.None);
        
        // Submit the job
        job.Submit();
    }



>[AZURE.NOTE]此代码段将加载顺序为简单起见，每个资产。 在生产中的环境资产将批量加载。 上传多个批量资产的详细信息请参阅[媒体服务 sdk.net 批量接收的资产](media-services-dotnet-upload-files.md#ingest_in_bulk)。

有关完整的代码示例[创建与媒体服务编码器的叠加](https://code.msdn.microsoft.com/Creating-Audio-and-Video-c2942c47)。  

###错误条件

在编辑预设字符串时，必须确保以下︰

- 对于视频/图像覆盖，覆盖矩形必须完全适合的源视频尺寸
- 叠加的开始和结束时间应该是在源视频的时间轴中
- 如果预设的 XML 包含的引用吗？OverlayFileName ="%n%"，则 InputAssets 集合的任务应包含至少 n + 1 资产



##缝合视频段

媒体服务编码器缝合在一起的一套视频提供支持。 视频可以缝合在一起端到端，也可以指定一个或两个视频来缝合在一起的部分。 缝合的结果是包含来自输入资产指定的视频的单个输出资产。 多项资产或一种单一资产中可包含视频，以进行缝合。 由传递到编码器的预设 XML 控制缝合。 预设的架构的完整说明，请参阅[Azure 媒体编码架构](https://msdn.microsoft.com/library/azure/dn584702.aspx)。 

###媒体服务编码器与缝合

缝合内控制<MediaFile>元素中的下列预设所示︰
    
    <MediaFile
        DeinterlaceMode="AutoPixelAdaptive"
        ResizeQuality="Super"
        AudioGainLevel="1"
        VideoResizeMode="Stretch">
        <Sources>
          <Source
            AudioStreamIndex="0">
            <Clips>
              <Clip
                StartTime="00:00:00"
                EndTime="00:00:05" />
            </Clips>
          </Source>
          <Source
            ResizeMode="Letterbox"
            ApplyCrop="False"
            AudioStreamIndex="0"
            MediaFile="%1%">
            <Clips>
              <Clip
                StartTime="00:00:00"
                EndTime="00:00:05" />
            </Clips>
          </Source>
          <Source
            ResizeMode="Letterbox"
            ApplyCrop="False"
            AudioStreamIndex="0"
            MediaFile="%2%">
            <Clips>
              <Clip
                StartTime="00:00:00"
                EndTime="00:00:05" />
            </Clips>
          </Source>
         </Sources>
    </MediaFile>

每个视频来进行缝合，<Source>元素添加到<Sources>元素。 每个<Source>元素中包含<Clips>元素。 每个<Clips>元素包含一个或多个<Clip>指定视频的元素将缝合到输出的资产，以通过指定开始和结束时间。 <Source>元素引用其作用相当的资产。 引用的格式取决于被缝合视频是否在独立的资产或在单个资产。 如果您想要将整个视频，只需省略<Clips>元素。 

###从多个资产的缝合视频

一个从零开始的索引，当缝合视频从多个资源，用于 MediaFile 特性<Source>元素，以标识哪些资产<Source>元素都对应。 未指定零索引， <Source> MediaFile 属性未指定的元素引用第一个输入的资产。 所有其他<Source>元素必须指定输入资产使用 %n%语法，其中 n 是输入资产的从零开始的索引引用的从零开始的索引。 在前面的示例中第一个<Source>元素指定第一个输入的资产，第二个<Source>元素指定第二个输入的资产，等等。 没有任何要求输入的资产被引用的顺序，例如︰
    
    <MediaFile
        DeinterlaceMode="AutoPixelAdaptive"
        ResizeQuality="Super"
        AudioGainLevel="1"
        VideoResizeMode="Stretch">
        <Sources>
          <Source
            AudioStreamIndex="0"
            MediaFile="%1%">
            <Clips>
              <Clip EndTime="00:00:05" 
                    StartTime="00:00:00" />
            </Clips>
                      
            </Source>
          <Source
           AudioStreamIndex="0">
            <Clips>
              <Clip
                StartTime="00:00:00"
                EndTime="00:00:05" />
            </Clips>
          </Source>
          <Source
              AudioStreamIndex="0"
             MediaFile="%2%">
            <Clips>
              <Clip
                StartTime="00:00:00"
                EndTime="00:00:05" />
            </Clips>
          </Source>
         </Sources>
    </MediaFile>

本示例装订在一起的第二个、 第一和第三个输入的资产部分。 请注意，因为从零开始的索引引用的每个视频很可能要缝合两个视频具有相同的名称。 一旦您已创建的预设的文件，您必须执行以下操作︰
 
- 上载您的资产
- 加载预设的配置
- 创建作业
- 获取引用媒体服务编码器
- 创建任务指定输入的资产、 预设的配置、 media 编码器中和输出资产
- 提交作业

下面的代码段说明如何执行这些步骤︰
    
    static public void StitchWithMultipleAssets()
    {
        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(
                        MediaServicesAccountName,
                        MediaServicesAccountKey);
        // Used the cached credentials to create CloudMediaContext.
        _context = new CloudMediaContext(_cachedCredentials);
        
        // Upload assets to stitch
        IAsset inputAsset1 = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, video1.mp4);
        IAsset inputAsset2 = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, video2.wmv);
        IAsset inputAsset3 = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, video3.wmv);
        
        // Load the preset configuration
        string presetFileName = "StitchingWithMultipleAssets.xml";
        string configuration = File.ReadAllText(presetFileName);
        
        // Create a job
        IJob job = _context.Jobs.Create("A AME stitching job, using " + presetFileName);
                 
        // Get a reference to the media services encoder   
        IMediaProcessor processor = GetLatestMediaProcessorByName("Azure Media Encoder");
            
        // Create a task    
        ITask task = job.Tasks.AddNew("Encode Task for stitching, using " + presetFileName, processor, configuration, TaskOptions.None);
        
        // Add the input assets
        task.InputAssets.Add(inputAsset1);
        task.InputAssets.Add(inputAsset2);
        task.InputAssets.Add(inputAsset3);
        
        // Add the output asset
        task.OutputAssets.AddNew("Result of a stitching job, using " + presetFileName, AssetCreationOptions.None);
        
        // Submit the job
        job.Submit();
        } 


此代码段将加载顺序为简单起见，每个资产。 在生产中的环境资产将批量加载。 上传多个批量资产的详细信息请参阅[媒体服务 sdk.net 批量接收的资产](media-services-dotnet-upload-files.md#ingest_in_bulk)。 有关完整的示例代码[与媒体服务编码器的 Stitching](https://code.msdn.microsoft.com/Stitching-with-Media-8fd5f203)。

###从单个资产的缝合视频

当缝合视频单个资产中的，每个视频必须具有唯一的名称。 这些视频是使用 MediaFile 属性使用文件名作为属性的值来指定的。 例如︰
    
    <MediaFile
        DeinterlaceMode="AutoPixelAdaptive"
        ResizeQuality="Super"
        AudioGainLevel="1"
        VideoResizeMode="Stretch">
        <Sources>
          <Source
            AudioStreamIndex="0"
            MediaFile="video1.mp4">
            <Clips>
              <Clip
                StartTime="00:00:00"
                EndTime="00:00:05" />
            </Clips>
          </Source>
          <Source
           AudioStreamIndex="0"
           MediaFile="video2.wmv">
    
            <Clips>
              <Clip
                StartTime="00:00:00"
                EndTime="00:00:05" />
            </Clips>
          </Source>
          <Source
              AudioStreamIndex="0"
             MediaFile="video3.wmv">
            <Clips>
              <Clip
                StartTime="00:00:00"
                EndTime="00:00:05" />
            </Clips>
          </Source>
         </Sources>
    </MediaFile>

此预设装订 video1.mp4、 video2.wmv 和 video3.wmv 的部分到输出资产。
创建作业和任务就是缝合视频从多个资源，只需上载单个资产，如以下代码所示︰

    // Creates a stitching job that uses a single asset 
    static public void StitchWithASingleAsset()
    {
        string presetFileName = "StitchingWithASingleAsset.xml";
        string configuration = File.ReadAllText(presetFileName);

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(
                        MediaServicesAccountName,
                        MediaServicesAccountKey);
        // Used the cached credentials to create CloudMediaContext.
        _context = new CloudMediaContext(_cachedCredentials);

        IMediaProcessor processor = GetLatestMediaProcessorByName("Azure Media Encoder");
        IJob job = _context.Jobs.Create("A AME stitching job, using " + presetFileName);
        IAsset asset = CreateAssetAndUploadMultipleFiles(AssetCreationOptions.None, _stitchingFiles);

        ITask task = job.Tasks.AddNew("Encode Task for stitching, using " + presetFileName, processor, configuration, TaskOptions.None);
        task.InputAssets.Add(asset);
        task.OutputAssets.AddNew("Result of a stitching job, using " + presetFileName, AssetCreationOptions.None);

        job.Submit();
    }

##编码演示文稿或音频流的主要是语音
 
编码的视频的音轨包含主要是语音，默认编码器预设可能会导致在编码资产放大背景噪音。 这种现象由 NormalizeAudio 属性设置为 true。

###与大部分语音编码演示文稿

若要防止背景噪音的放大，执行以下操作︰

1. 编码器的内容复制到 XML 文件中使用的预设。 可以在上找到编码器预设︰ Azure 媒体编码架构
1. 删除 NormalizeAudio 属性，可以通过在预设文件的顶部附近发现<MediaFile>元素︰
    
    <MediaFile
         DeinterlaceMode="AutoPixelAdaptive"
         ResizeQuality="Super"
         NormalizeAudio="True"
         AudioGainLevel="1"
         VideoResizeMode="Stretch">

1. 修改预设的文件保存到本地硬盘，然后使用如下所示的代码来使用自定义预设编码︰
        
        // Upload file and create asset
        IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, @"C:\TEMP\Original.mp4");
         
        string inputPresetFile = @"C:\TEMP\H264 Broadband 720p NoAudioNorm.xml";
        string presetName = Path.GetFileNameWithoutExtension(inputPresetFile);
         
        IJob job = _context.Jobs.Create("Encode Job for " + asset.Name + ", encoded using " +  presetName);
        
        Console.WriteLine("Encode Job for " + asset.Name + ", encoded using " + presetName);
        
        // Get a media processor reference, and pass to it the name of the processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Azure Media Encoder");
        Console.WriteLine("Got MP " + processor.Name + ", ID : " + processor.Id + ", version: " + processor.Version);
         
        // Read the configuration data into a string. 
        string configuration = File.ReadAllText(inputPresetFile);
         
        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("Encode Task for " + asset.Name + ", encoded using " + presetName, processor, configuration,
                        Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);
         
        // Specify the input asset to be encoded.
        task.InputAssets.Add(asset);
         
        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("Output asset for " + asset.Name + ", encoded using " + presetName, AssetCreationOptions.None);
         
        // Launch the job. 
        job.Submit();

##请参见

[Azure 媒体编码 XML 架构](https://msdn.microsoft.com/library/azure/dn584702.aspx)测试
