---
ms.openlocfilehash: 7c90d2d4f4a32ee658348216d0ef94ba97db8745
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="管理门户用于创建从内部编码器接收多比特率实时流的通道"
    description="本教程将带领您一步步地实施基本的介质服务实时流应用程序的其中一个通道从内部实时编码器接收多比特率的实时流。"
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
    ms.topic="get-started-article" 
    ms.date="08/11/2015" 
    ms.author="juliako"/>


#管理门户用于创建从内部编码器接收多比特率实时流的通道

[AZURE.INCLUDE [media-services-selector-manage-channels](../../includes/media-services-selector-manage-channels.md)]


本教程将带领您一步步地实施基本的介质服务实时流应用程序的**通道**从内部实时编码器接收多比特率实时流的位置。 使用通道和相关的组件的更详细概览，请参阅[该接收多通道处理的比特率从内部编码器的实时流](media-services-manage-channels-overview.md)。

在本教程中，使用 Azure 管理门户来完成下列任务︰

2.  配置流式处理的终结点
3.  创建通道
1.  配置实时编码器，并接收到 （在此步骤中使用 Wirecast） 的通道实时流
1.  创建一个程序 （和资产）
1.  发布资产并获取流的 Url  
1.  播放您的内容
2.  清理

##先决条件
完成本教程需要以下。

- 若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。
有关详细信息，请参阅[Azure 免费试用版](azure.microsoft.com)。
- 媒体服务帐户。 若要创建的介质服务帐户，请参阅[创建帐户](media-services-create-account.md)。
- 网络摄像机，并可以发送多比特率的编码器的实时流。


##配置使用门户的流终结点

当使用 Azure 媒体服务，一个最常见的方案提供了自适应流式处理到您的客户端的比特率。 具有自适应的比特率传输，客户端可以切换到一个更高或更低的比特率流视频的显示基于当前网络带宽、 CPU 利用率和其他因素。 媒体服务支持流式技术下自适应比特率︰ HTTP 实时流 (HLS)、 平滑流式处理、 MPEG 划线和 HDS （对 Adobe 至此/访问许可证持有者只）。

在使用实时流，内部实时编码器 （在我们的案例 Wirecast) 到您的渠道摄取多比特率的实时流。 当用户请求流时，介质服务使用动态打包到请求的自适应比特率流 （HLS、 短划线或光滑） 重新打包的源流。

要充分利用动态打包，需要获得至少一个流单元**流终结点**从该计划传递给您的内容。

要更改流保留的单位的数量，请执行以下操作︰

1. 在[管理门户网站](https://manage.windowsazure.com/)中，单击**媒体服务**。 然后，单击媒体服务的名称。

2. 选择流终结点页。 然后，单击想要修改的流终结点。

3. 若要指定流的单位数，缩放比例选项卡中选择和移动滑块**预留的产能**。

    ![缩放页面](./media/media-services-portal-get-started-with-live/media-services-origin-scale.png)

4. 按保存按钮保存所做的更改。

    任何新单位的分配采用大约 20 分钟的时间才能完成。


    >[AZURE.NOTE] 目前，正在从任何正流回无单位的值，可以禁用流的长达一个小时。
    >
    > 单位指定的 24 小时内的最高数目用于计算成本。 有关定价的详细信息，请参阅[媒体服务定价详细信息](http://go.microsoft.com/fwlink/?LinkId=275107)。


##创建通道

在 Azure 管理门户中，选择**频道**页。 然后，单击**新建**。 在**创建一个新的实时通道**对话框中输入您的频道的名称。

![createchannel](./media/media-services-portal-get-started-with-live/media-services-create-channel.png)

按**确定**。

几分钟后通道获取创建并启动。

##获取接收 Url

创建通道后，就可以接收到实时编码器提供的 Url。 编码器使用这些 Url 输入实时流。

![readychannel](./media/media-services-portal-get-started-with-live/media-services-ready-channel.png)


![ingesturls](./media/media-services-portal-get-started-with-live/media-services-ingest-urls.png)

有关接收 Url 的详细信息，请参阅[使用内部编码器发送到通道多比特率实时流](../media-services-channels-overview.md)。

##配置实时编码器和接收实时流

>[AZURE.NOTE]此步骤需要的通道接收在上一步中提到的 URL。

有关如何配置 Wirecast 和启动接收流的详细信息，请参阅[Wirecast 配置](http://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)。

>[AZURE.NOTE]如果出于任何原因停止编码器，并需要重新启动它应首先通过单击 Azure 管理门户中的**重置**命令置通道。


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

    ![createprogram](./media/media-services-portal-get-started-with-live/media-services-create-program.png)

    如果您**发布此程序现在**选中，将创建程序发布 Url。

    可以按**开始**，流程序已准备好的时候。

    一旦您启动该程序时，您可以按播放开始播放内容。


    ![createdprogram](./media/media-services-portal-get-started-with-live/media-services-created-program.png)

2. 或者，可以使用快捷方式，并按**频道**页上**开始流**按钮。 这将创建资产、 程序及流定位器。

    程序被命名为 DefaultProgram 并存档窗口设置为 1 小时。

    您可以播放频道页面发布的程序。

    ![channelpublish](./media/media-services-portal-get-started-with-live/media-services-channel-play.png)


如果**频道**页上单击**停止流**，将停止并删除默认程序。 资产，仍然会存在，可以发布或取消发布该**内容**的页。

如果您切换到**内容**页，您将看到您的程序所创建的资产。

![contentasset](./media/media-services-portal-get-started-with-live/media-services-content-assets.png)


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



<!-- URLs. -->
[管理门户]: http://manage.windowsazure.com/

<!-- Images -->
 

测试
