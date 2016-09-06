---
ms.openlocfilehash: 1c77179b4e33d8bd4b38434db02476dc35b0fc83
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="提供使用.NET SDK 的视频点播 (VoD) 内容入门"
    description="本教程将带领您逐步完成使用 Azure 媒体服务使用.NET 实现视频点播 (VoD) 内容交付应用程序。"
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
    ms.topic="hero-article"
    ms.date="08/18/2015"
    ms.author="juliako"/>


# 提供使用.NET SDK 的视频点播 (VoD) 内容入门

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> 若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅<a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure 免费试用版</a>。

本教程将带领您一步步地实施针对.NET 使用 Azure 媒体服务 (AMS) SDK 的视频点播 (VoD) 内容交付应用程序。


本教程介绍了基本的媒体服务工作流的最常见的编程对象和所需的介质服务开发任务。 在完成本教程后，您将能够流或渐进式下载一个示例媒体文件上载，编码，并下载。  

## 先决条件
启动为.NET 开发与媒体服务 SDK 需要以下系统必备组件。

- 操作系统︰ Windows 8 或更高版本，Windows 2008 R2，Windows 7。
- .NET framework 4.5 或.NET Framework 4.0
- Visual Studio 2013年，Visual Studio 2012，Visual Studio 2010 SP1 （专业、 特优、 终极，或速成版）。


下列任务所示此快速入门。

1.  创建介质服务帐户 （使用 Azure 门户）。
2.  配置 （使用门户网站） 的流终结点。
3.  创建和配置 Visual Studio 项目。
5.  连接到媒体服务帐户。
6.  创建新资产并上传视频文件。
7.  编码为一组自适应比特率 MP4 文件的源文件。
8.  发布资产和获得流式和渐进式下载 Url。  
9.  播放您的内容。


##创建介质服务帐户使用门户

1. 在 Azure 的门户中，单击**新建**，单击**媒体服务**，然后单击**快速创建**。

    ![媒体服务快速创建](./media/media-services-dotnet-get-started/wams-QuickCreate.png)

2. 在**名称**框中，输入新帐户的名称。 媒体服务帐户名称全部小写字母数字或字母，并且不包含空格，并且 3-24 个字符长。

3. 在**区域**中，选择将用于存储介质服务帐户的元数据记录的地理区域。 只有可用的介质服务区域出现在下拉列表中。

4. 在**存储帐户**，选择存储帐户提供媒体内容的媒体服务帐户的 blob 存储。 您可以选择现有的存储帐户在同一地理区域作为介质服务帐户，也可以创建新的存储帐户。 在同一区域中创建新的存储帐户。

5. 如果您创建一个新的存储帐户，在**新的存储帐户名称**中，输入存储帐户的名称。 存储帐户名称的规则是与媒体服务帐户相同。

6. 单击**快速创建**表单的底部。

    您可以监视窗口底部的消息区域中的进程的状态。

    已成功创建您的帐户后，状态将更改为**活动**。

    在页面的底部，将出现**管理密钥**按钮。 当您单击此按钮时，将显示与媒体服务帐户名和主、 辅键进行对话。 您需要的帐户名和主键信息以编程方式访问该介质服务帐户。

    ![媒体服务页](./media/media-services-dotnet-get-started/wams-mediaservices-page.png)

    双击帐户名称时，默认情况下显示**快速启动**页。 此页使您能够执行一些管理任务，也是门户网站的其他网页。 例如，可以上载视频文件从该页面也可以做到从内容页。

##配置使用门户的流终结点

当使用 Azure 媒体服务，一个最常见的方案提供了自适应流式处理到您的客户端的比特率。 具有自适应的比特率传输，客户端可以切换到一个更高或更低的比特率流视频的显示基于当前网络带宽、 CPU 利用率和其他因素。 媒体服务支持流式技术下自适应比特率︰ HTTP 实时流 (HLS)、 平滑流式处理、 MPEG 划线和 HDS （对 Adobe 至此/访问许可证持有者只）。

媒体服务提供动态打包以便您可以传递中流媒体服务 MPEG 划线、 HLS、 平滑流式处理 （HDS） 支持的格式的 MP4 或平滑流编码的自适应比特率内容而不必对这些流格式重新打包。

利用动态打包，您需要执行下列操作︰

- 编码或编码转换成自适应比特率 MP4 文件或自适应的比特率平滑流式的文件 （编码步骤演示本教程的后面），一组文件的您夹层 （源）  
- **流终结点**从该计划传递给您的内容获取至少一个流的计价单位。

通过动态打包，您只需将存储并支付中单个存储格式的文件，介质服务将生成和提供适当的响应，基于客户机的请求。

要更改流保留的单位的数量，请执行以下操作︰

1. 在[门户网站](https://manage.windowsazure.com/)中，单击**媒体服务**。 然后，单击媒体服务的名称。

2. 选择流终结点页。 然后，单击想要修改的流终结点。

3. 若要指定流的单位数，单击刻度选项卡，然后移动滑块**预留的产能**。

    ![缩放页面](./media/media-services-dotnet-get-started/media-services-origin-scale.png)

4. 按**保存**以保存所做的更改。

    任何新单位的分配采用大约 20 分钟的时间才能完成。

    >[AZURE.NOTE] 目前，从任何正值流回无单位的可以禁用流的长达一个小时。
    >
    > 单位指定的 24 小时内的最高数目用于计算成本。 有关定价的详细信息，请参阅[媒体服务定价的详细信息](http://go.microsoft.com/fwlink/?LinkId=275107)。



##创建和配置 Visual Studio 项目

1. 在 Visual Studio 2013年，Visual Studio 2012 或 Visual Studio 2010 SP1 中创建一个新 C# 控制台应用程序。 输入**名称**、**位置**和**解决方案名称**，然后单击**确定**。

2. 使用[windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) Nuget 程序包安装**Azure 媒体服务.NET SDK 扩展**。  媒体服务.NET SDK 扩展为扩展方法和帮助程序函数，简化您的代码更容易地使用介质服务开发一套。 安装此软件包，也将安装**介质服务.NET SDK**并添加其他所需的所有依赖项。

3. 添加 System.Configuration 程序集的引用。 此程序集包含用于访问配置文件中，例如，App.config 的**System.Configuration.ConfigurationManager**类。

4. 打开 App.config 文件 （如果未添加默认情况下，则将文件添加到您的项目中） 并将*appSettings*部分添加到该文件。 设置 Azure 媒体服务帐户的名称和帐户键，值，如下面的示例中所示。 获取的帐户名称和密钥信息，打开 Azure 的门户网站，选择介质服务帐户，然后单击**管理密钥**按钮。

     <pre><code>
    &lt;configuration&gt;
        &lt;appSettings&gt;
        &lt;add key="MediaServicesAccountName" value="Media-Services-Account-Name" /&gt;
            &lt;add key="MediaServicesAccountKey" value="Media-Services-Account-Key" /&gt;
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
    </code></pre>

5. 用下面的代码覆盖现有**using**语句开头的 Program.cs 文件。

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

6. 在项目目录下新建一个文件夹并将您想要进行编码并流或渐进式下载.mp4 或.wmv 文件复制。 在此示例中，使用"C:\VideoFiles"路径。

##连接到媒体服务帐户

当使用.NET 的介质服务，必须使用**CloudMediaContext**类的大多数编程任务的介质服务︰ 连接到媒体服务帐户;创建、 更新、 访问和删除以下对象︰ 资产、 资源文件、 作业、 访问策略、 定位器等。

用下面的代码覆盖默认的计划类别。 该代码演示如何从 App.config 文件中读取连接值，以及如何创建**CloudMediaContext**对象，以便连接到介质服务。 有关连接到介质服务的详细信息，请参阅[连接到使用.NET 媒体服务 SDK 的媒体服务](http://msdn.microsoft.com/library/azure/jj129571.aspx)。

**Main**函数调用将定义的方法进一步在这一节。

    class Program
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;

        static void Main(string[] args)
        {
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##创建新资产并上传视频文件

媒体服务中您上载 （或接收） 到资产数字文件。 **资产**实体可以包含视频、 音频、 图像、 缩略图集合，文字轨道，和隐藏的字幕文件 （以及有关这些文件的元数据。） 一旦上载文件的则中以便进行进一步的处理和流云安全地存储您的内容。 在资产中的文件被称为**资产文件**。

以下调用**CreateFromFile** （定义.NET SDK 扩展中） 定义的**UploadFile**方法。 **CreateFromFile**创建在其中上载指定的源文件的新资产。

**CreateFromFile**方法采用**AssetCreationOptions** ，您可以指定以下资产创建选项之一︰

- 使用**无**-不加密。 这是默认值。 注意，使用此选项时，您的内容不受在传输过程中或在存储中存放。
如果您打算提供 MP4 使用渐进式下载，请使用此选项。
- **StorageEncrypted** -使用此选项来加密明文内容使用高级加密标准 (AES)-256 位加密，然后将其上载到的存储位置的 Azure 存储本地加密存放。 未自动加密和放在编码之前加密的文件系统使用存储加密保护的资产，而且 （可选） 重新加密之前上载回作为一个新的输出资产。 您想要高质量输入的媒体文件使用强加密存放在磁盘上安全时，存储加密的主用例。
- **CommonEncryptionProtected** -使用此选项，如果您要上载已加密和常见的加密或 PlayReady （例如，平滑流式处理保护与 PlayReady DRM） 的 DRM 保护的内容。
- **EnvelopeEncryptionProtected** – 使用此选项，如果您要上载 HLS 使用 AES 加密。 请注意，文件必须具有已编码加密转换管理器。

**CreateFromFile**方法，还可以指定回调以便上载文件的进度报告。

在以下示例中，我们指定**无**资产选项。

将下面的方法添加到程序类。

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


##编码为一组自适应比特率 MP4 文件的源文件

后到介质服务接收资产，媒体可以是编码的 transmuxed，水印，依此类推之前它被传递到客户端。 这些活动在计划和运行多个背景角色实例，以确保高性能和可用性。 这些活动被称为作业，和每个作业组成原子任务的执行上资源文件的实际工作。

如前文所述，在使用 Azure 媒体服务时，一种最常见的方案提供了自适应流式处理到您的客户端的比特率。 介质服务可以动态打包的一组自适应比特率 MP4 文件到以下格式之一︰ HTTP 实时流 (HLS)、 平滑流式处理、 MPEG 划线和 HDS （对 Adobe 至此/访问许可证持有者只）。

利用动态打包，您需要执行下列操作︰

- 编码或编码转换成一组自适应比特率 MP4 文件或自适应的比特率平滑流式文件您夹层 （源） 的文件。  
- 从中您计划交付给您的内容的流终结点获取至少一个流的计价单位。

下面的代码演示如何将编码作业提交。 该作业包含指定编码转换到夹层文件到一套自适应比特率 MP4s 使用**Azure Media 编码器**的一个任务。 该代码提交作业，并一直等待，直到它完成。

完成作业后，将能够传输您的资产或渐进式下载转码的结果创建的 MP4 文件。
请注意，您不需要具有多 0 个流设备才能渐进式下载 MP4 文件。

将下面的方法添加到程序类。

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.

        IJob job = _context.Jobs.CreateWithSingleTask(
            MediaProcessorNames.AzureMediaEncoder,
            MediaEncoderTaskPresetStrings.H264AdaptiveBitrateMP4Set720p,
            asset,
            "Adaptive Bitrate MP4",
            options);

        Console.WriteLine("Submitting transcoding job...");


        // Submit the job and wait until it is completed.
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

##发布资产和 Url 获取流和渐进式下载

流或下载资产，首先需要将其"发布"通过创建定位程序。 定位器提供资产中包含的文件的访问权限。 媒体服务支持两种类型的定位器︰ OnDemandOrigin 定位器，用于流媒体 （例如，MPEG 划线、 HLS，或平滑流式处理） 和访问签名 (SAS) 定位程序，用来下载媒体文件。

创建定位程序后，您可以构建用于流或下载文件的 Url。


平滑流式处理的流式处理 URL 具有以下格式︰

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS 流 URL 具有以下格式︰

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG 划线的流式处理 URL 具有以下格式︰

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

SAS URL 用于下载文件具有以下格式︰

    {blob container name}/{asset name}/{file name}/{SAS signature}

媒体服务.NET SDK 扩展提供方便的帮助器方法返回格式化的 Url 已发布资产。

下面的代码使用.NET SDK 扩展来创建定位程序并获取流和渐进式下载 Url。 该代码还演示如何将文件下载到本地文件夹。

将下面的方法添加到程序类。

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##播放您的内容  

一旦运行在前一节中定义的程序，类似于下面的 Url 将显示在控制台窗口中。

自适应流式处理的 Url:

平滑流式处理

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG 短划线

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

渐进式下载 Url （音频和视频）。

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


以流式传输视频，使用[Azure 服务媒体播放](http://amsplayer.azurewebsites.net/azuremediaplayer.html)。

若要测试渐进式下载，粘贴到浏览器中 （例如，Internet Explorer，镶边或 Safari） 的 URL。

##下一步

若要了解有关构建视频点播应用程序的详细信息，请参阅[生成 VoD 应用程序](media-services-video-on-demand-workflow.md)。

###其他资源
- <a href="http://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-101-Get-your-video-online-now-">Azure 媒体服务课程 101-立即获取在线视频 ！</a>
- <a href="http://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-102-Dynamic-Packaging-and-Mobile-Devices">Azure 媒体服务 102-动态打包和移动设备</a>


<!-- Anchors. -->


<!-- URLs. -->
  [Web 平台安装程序]: http://go.microsoft.com/fwlink/?linkid=255386
  [门户]: http://manage.windowsazure.com/

测试
