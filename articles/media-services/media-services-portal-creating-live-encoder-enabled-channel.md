---
ms.openlocfilehash: 6cac8e38bed9e6f72c14d4f087817ef4506a8812
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="管理门户用于创建执行实时编码到一个比特率的频道多比特率流" 
    description="本教程将指导您逐步完成创建接收单比特率实况流并将其编码到多比特率流通道。" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="08/11/2015"
    ms.author="juliako"/>


#管理门户用于创建执行实时编码单比特率对多通道的比特率流 （预览）

> [AZURE.SELECTOR]
- [门户](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET SDK](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API，](https://msdn.microsoft.com/library/azure/dn783458.aspx)

本教程将指导您逐步完成创建**通道**接收单比特率实况流并将其编码到多比特率流。

>[AZURE.NOTE]启用实时编码的渠道与有关的概念信息，请参阅[与渠道合作，执行实时编码到一个比特率从多比特率流](media-services-manage-live-encoder-enabled-channels.md)。

##常见的实时流方案

下面是创建通用实时流的应用程序所涉及的一般步骤。

1. 将摄像机连接到计算机。 启动和配置内部实时编码器，可以输出一个比特率流中下列协议之一︰ RTMP、 平滑流式处理或 RTP (MPEG-TS)。 有关详细信息，请参阅[Azure 媒体服务 RTMP 支持和实时编码器](http://go.microsoft.com/fwlink/?LinkId=532824)。
    
    创建您的频道后，还可以执行此步骤。

1. 创建并启动一个通道。 

1. 检索该信道接收 URL。 

    实时编码器使用接收 URL 发送流通道。
1. 检索通道预览 URL。 

    使用此 URL 来验证您的频道正确接收实时流。

3. 创建一个程序 （还将创建资产）。 
1. 发布的程序 （将创建关联的资产按需定位符）。  

    请确保至少有一个流保留的单位要流内容的流终结点。
1. 当准备好开始流式处理和归档时启动的程序。
2. （可选） 可以实时编码器发出信号，启动一个广告。 在输出流中插入广告。
1. 如果想要停止流处理和归档事件，停止程序。
1. 删除程序 （还可以选择删除资产）。   

##在本教程中

在本教程中，使用 Azure 管理门户来完成下列任务︰ 

2.  配置流式处理的终结点。
3.  创建一个通道，启用进行实时编码。
1.  为了提供实时编码器获得摄取 URL。 实时编码将使用此 URL 接收通道中的流。 .
1.  创建一个程序 （和资产）
1.  发布资产并获取流的 Url  
1.  播放您的内容 
2.  清理

##先决条件
完成本教程需要以下。

- 若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](azure.microsoft.com)。
- 媒体服务帐户。 若要创建的介质服务帐户，请参阅[创建帐户](media-services-create-account.md)。
- 网络摄像机，可以发送一个比特率的编码器的实时流。

##配置使用门户的流终结点

当使用 Azure 媒体服务，一个最常见的方案提供了自适应流式处理到您的客户端的比特率。 具有自适应的比特率传输，客户端可以切换到一个更高或更低的比特率流视频的显示基于当前网络带宽、 CPU 利用率和其他因素。 媒体服务支持流式技术下自适应比特率︰ HTTP 实时流 (HLS)、 平滑流式处理、 MPEG 划线和 HDS （对 Adobe 至此/访问许可证持有者只）。 

在使用实时流，内部实时编码器 （在我们的案例 Wirecast) 到您的渠道摄取多比特率的实时流。 当用户请求流时，介质服务使用动态打包到请求的自适应比特率流 （HLS、 短划线或光滑） 重新打包的源流。 

要充分利用动态打包，需要获得至少一个流单元**流终结点**从该计划传递给您的内容。

要更改流保留的单位的数量，请执行以下操作︰

1. 在[管理门户网站](https://manage.windowsazure.com/)中，单击**媒体服务**。 然后，单击媒体服务的名称。

2. 选择流终结点页。 然后，单击想要修改的流终结点。

3. 若要指定流的单位数，缩放比例选项卡中选择和移动滑块**预留的产能**。

    ![缩放页面](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-origin-scale.png)

4. 按保存按钮保存所做的更改。

    任何新单位的分配采用大约 20 分钟的时间才能完成。 

     
    >[AZURE.NOTE] 目前，正在从任何正流回无单位的值，可以禁用流的长达一个小时。
    >
    > 单位指定的 24 小时内的最高数目用于计算成本。 有关定价的详细信息，请参阅[媒体服务定价详细信息](http://go.microsoft.com/fwlink/?LinkId=275107)。

 
##创建通道

1.  在[管理门户网站](http://manage.windowsazure.com/)中，单击媒体服务，然后单击介质服务帐户名称。
2.  选择频道页。
3.  选择添加 + 添加一个新的通道。

选择**标准**编码类型。 这种类型指定您想要创建一个通道，可用于实时编码。 这意味着传入通道发送一个比特率流并将其编码到多比特率流使用指定实时编码器设置。 有关详细信息，请参阅[与渠道合作，执行实时编码到一个比特率从多比特率流](media-services-manage-live-encoder-enabled-channels.md)。

![standard0][standard0]

对于**标准**编码类型，有效接收的协议选项是︰

- 单零碎 MP4 的比特率 （平滑流式处理）
- 一个比特率 RTMP
- RTP (MPEG-TS): 通过 RTP 的 mpeg-2 传输流。

有关每个协议的详细说明，请参见[与渠道合作，执行实时编码到一个比特率从多比特率流](media-services-manage-live-encoder-enabled-channels.md)。

![standard1][standard1]

您不能更改通道时输入的协议或其关联的程序正在运行。 如果您需要不同的协议，应创建单独的通道，为每个输入协议。  

在**广告配置**页面可以指定 ad 标记信号源。 在使用门户网站时，您只能选择 API，这表示收听实时编码器在渠道内的异步的 Ad 标记 API。 在使用门户网站时，您只能选择 API。

有关详细信息，请参阅[与渠道合作，执行实时编码到一个比特率从多比特率流](media-services-manage-live-encoder-enabled-channels.md)。

![standard2][standard2]

在**预设编码**页面上，您可以选择系统预设。 目前，只有系统预设，您可以选择是**默认 720p**。

![standard3][standard3]

在**通道创建**页上，您可以定义允许将视频发布到此通道的 IP 地址。 允许的 IP 地址可以被指定为单个 IP 地址 (例如"10.0.0.1')、 IP 范围内使用的 IP 地址和一个 CIDR 子网掩码 (例如"10.0.0.1/22") 或使用 IP 地址和一个点分十进制子网掩码的 IP 范围 (如 10.0.0.1(255.255.252.0)')。

如果不指定了任何 IP 地址且没有规则定义将不允许任何 IP 地址。 允许任何 IP 地址，请创建一个规则，将 0.0.0.0/0。


![standard4][standard4]

>[AZURE.NOTE] 当前在预览中，通道开始可能需要 30 分钟。 通道重置可能需要最多 5 分钟。

一旦创建通道时，您可以选择**编码**选项卡可以在其中查看您的通道配置。 您还可以管理广告和平板电脑。 

![standard5][standard5]

有关详细信息，请参阅[与渠道合作，执行实时编码到一个比特率从多比特率流](media-services-manage-live-encoder-enabled-channels.md)。


##获取接收 Url

创建通道后，就可以接收到实时编码器提供的 Url。 编码器使用这些 Url 输入实时流。

![readychannel](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ready-channel.png)


![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##创建和管理程序

###概述

频道是与程序，使您可以控制发布和实时流中的段数的存储相关联。 渠道管理程序。 通道和程序的关系是内容的非常类似于传统媒体在其中的通道具有恒定流和程序的作用范围是内容的该频道在一些定时事件。

您可以指定要保留的程序录制的内容通过设置**存档窗口**长度的小时数。 此值可以设置从至少 5 分钟，最大为 25 小时。 归档文件窗口长度还规定时间客户端的最大数量可以在寻求从当前的实时位置。 程序可以运行在指定数量的时间，但后面的窗口长度范围的内容不断地被丢弃。 此属性的该值还决定多长时间可以扩大客户端清单。

每个程序都与资产相关联。 发布程序必须创建相关联的资产的按需定位器。 无此定位器将使您能够生成您可以提供给您的客户端的流式处理 URL。

一个通道支持最多三个同时运行的程序，以便您可以创建多个归档文件的同一个传入流。 这使您能够发布和存档的事件的不同部分，根据需要。 例如，您的业务要求是存档程序、 6 个小时，但广播只最近 10 分钟之内。 为此，您需要创建两个同时运行的程序。 一个程序被设置为存档 6 小时的事件，但不是发布程序。 其他程序设置为 10 分钟存档并发布该程序。

您不应该重用现有程序的新事件。 相反，创建并启动新程序为每个事件编程实时数据流的应用程序一节中所述。

当准备好开始流式处理和归档时启动的程序。 如果想要停止流处理和归档事件，停止程序。 

要删除归档的内容，请停止并删除该程序然后删除关联的资产。 无法删除资产，如果它由一个程序;必须首先删除该程序。 

即使停止和删除程序后，用户将能够为视频点播、 为流归档的内容，只要不删除资产。

如果您想保留存档的内容，但不是让其可进行流式处理，删除流式定位器。

###创建/启动/停止应用程序

一旦流入通道流可以通过创建资产、 程序及流定位器开始流式处理的事件。 将存档流并将其提供给浏览者通过流式传输终结点。 

有两种方法启动事件︰ 

1. 从**通道**页中，按**添加**要添加新程序。

    指定︰ 程序名称、 资源名称、 存档窗口和加密选项。
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    如果您**发布此程序现在**选中，将创建程序发布 Url。
    
    可以按**开始**，流程序已准备好的时候。

    一旦您启动该程序时，您可以按播放开始播放内容。


    ![createdprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-created-program.png)

2. 或者，可以使用快捷方式，并按**频道**页上**开始流**按钮。 这将创建资产、 程序及流定位器。

    程序被命名为 DefaultProgram 并存档窗口设置为 1 小时。

    您可以播放频道页面发布的程序。 

    ![channelpublish](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-channel-play.png)


如果**频道**页上单击**停止流**，将停止并删除默认程序。 资产，仍然会存在，可以发布或取消发布该**内容**的页。

如果您切换到**内容**页，您将看到您的程序所创建的资产。

![contentasset](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-content-assets.png)


##播放内容

为您的用户提供可用于传输您的内容的 URL，您首先需要"发布"您的资产 （如已在上一节中所述） 通过创建定位程序 （发布资产使用门户时，定位程序会为您创建）。 定位器提供资产中包含的文件的访问权限。 

根据您想要使用播放您的内容哪些流协议，您可能需要修改可获得 channel\program 的**发布 URL**链接的 URL。

动态打包将负责打包实时流到指定的协议。 

默认情况下，流 URL 具有以下格式，可以使用它来播放平滑流式处理的资产︰

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

若要生成流的 URL HLS，追加 (格式 = m3u8 aapl) 的 url。

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

若要生成流的 URL 为 MPEG 划线，追加 (格式 = mpd-时间-csf) 的 url。

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

提供您的内容的详细信息，请参阅[多语言内容](media-services-deliver-content-overview.md)。

您可以播放平滑流使用[AMS 的播放机](http://amsplayer.azurewebsites.net/azuremediaplayer.html)或使用 iOS 和 Android 设备播放 HLS 版本 3。

##清理

如果完成流式处理事件并且想清理前面提供的资源，请按照下面的过程。

- 停止从编码器推送流。
- 停止通道。 一旦停止通道，它将不会导致任何费用。 如果需要重新启动，它将具有相同的摄取的 URL，这样您无需重新配置您的编码器。
- 您可以停止流终结点，除非您想要继续提供实时点播流作为活动归档。 如果通道处于停止状态时，它将不会导致任何费用。
  


[standard0]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard0.png
[standard1]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard1.png
[standard2]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard2.png
[standard3]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard3.png
[standard4]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard4.png
[standard5]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard_encode.png 
测试t
