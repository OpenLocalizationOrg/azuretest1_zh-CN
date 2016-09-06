---
ms.openlocfilehash: 2db76cea80f01053601f35c395cbb279f32b24f4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure RemoteApp 常见问题解答" 
    description="Azure 远程应用程序有关的常见问题。" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/06/2015" 
    ms.author="elizapo"/>

# Azure RemoteApp 常见问题解答
我们已经听到有关 Azure RemoteApp 的下面的问题。 有其他人？ 访问[远程应用程序论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp)，让我们知道您需要知道。

## Azure RemoteApp 是什么？ ##


- **Azure RemoteApp 是什么？** RemoteApp 是 Azure 服务可帮助您从许多不同的用户设备向应用程序提供安全的远程访问。 阅读有关[Azure 远程应用程序](remoteapp-whatis.md)详细信息。
- **两种部署选项有哪些？** 有两种类型的远程应用程序部署 （或集合）︰ 云和混合。 猜出[部署选项](remoteapp-whatis.md)工作最适合您的组织。

## 受支持的配置 ##

- **服务限制有哪些？** 您可以将默认设置和服务限制在[Azure 订阅和服务限制、 配额和限制](.\azure-subscription-service-limits.md)的 Azure RemoteApp 的了解。 让我们知道是否您有更多的问题。
- **有多少用户有？** 还有不少于 20 个用户。 让我重复，super 清楚-最小值为 20。 您将收取 20。 
- **支持自定义的业务线 (LOB) 应用程序？** 是。 在 Azure 远程应用程序中使用自定义应用程序，创建一个[自定义模板图像](remoteapp-create-custom-image.md)，然后将其上载到远程应用程序集合。
- **我自定义的 LOB 应用程序可以在 Azure RemoteApp 上工作吗？** 图是测试它出去的最佳方法。 检查[应用程序兼容性的要求](http://www.microsoft.com/download/details.aspx?id=18704)，签出[研发兼容性中心](http://www.rdcompatibility.com/compatibility/default.aspx)。
- **何种部署方法 （云或混合） 是最适合我的组织？** 如果您想与单一登录 (SSO) 的完全集成和内部部署安全的网络连接，则混合集合提供最完整的体验。 云的集合提供灵活方便地找出您的部署使用多个身份验证方法。 阅读有关[部署选项](remoteapp-whatis.md)的详细信息。
- **该混合集合要求 VNET。 我们可以使用我们现有的 VNET 吗？** 可以根据现有的 VNET 是 Azure VNET。 请参阅"步骤 1︰ 设置虚拟网络"中[混合收集说明](remoteapp-create-hybrid-deployment.md)的详细信息。
- **可以使用云或现有的虚拟机作为模板的 RemoteApp 收藏？** 是的！ 可以创建基于 Azure VM 映像，使用一幅图像包含在您的订阅，或创建自定义图像。 签出[RemoteApp 图像选项](remoteapp-imageoptions.md)。
- **我们拥有 SQL 或另一个数据库或者内部部署或在 Azure 中。 哪一种部署类型，我们应当使用？** 这取决于您的 SQL 或后端数据库所在。 如果数据库是在专用网络中，使用混合集合。 如果暴露给 Internet 并允许客户端连接来连接到该数据库的情况下，您可以使用云集合。
- **驱动器映射、 USB 和串行端口、 剪贴板共享，和打印机重定向呢？** 所有这些功能都支持 Azure 远程应用程序。 默认情况下启用剪贴板共享和打印机重定向。 您可以了解更多有关重定向[此处](remoteapp-redirection.md)。 


- **如何验证？ 支持哪些方法？** 云集合支持 Microsoft 帐户和 Azure Active Directory 帐户，以及 Office 365 提供帐户。 混合集合支持进行同步 （使用类似[Azure 活动目录同步](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)工具） 的 Azure Active Directory 帐户从 Windows 服务器的 Active Directory 部署;具体而言，或者使用密码同步选项同步与 Active Directory 联合身份验证服务 (AD FS) 联合身份验证配置同步。 您还可以配置[多因素身份验证 (MFA)](../../services/multi-factor-authentication/)。

    **注意︰**Azure Active Directory 用户必须在与您的订阅的租户。 （您可以查看和修改您在门户中的**设置**选项卡上的订阅。 请参阅[更改 Azure Active Directory 租户使用远程应用程序](remoteapp-changetenant.md)的详细信息）。

- **为什么不能让我 Azure Active Directory 帐户访问权限？** Azure Active Directory 用户必须具有与订阅相关的目录中。 您可以查看或修改该目录在门户中的设置选项卡上。 有关更多信息，请参见[更改 Azure Active Directory 租户使用远程应用程序](remoteapp-changetenant.md)。
- **哪些设备和操作系统的客户端应用程序支持？** 客户端应用程序是可用于 Windows 8.1、 Windows 8，Windows 7 Service Pack 1，iOS、 Mac OS X，Windows RT，Android 设备和 Windows Phone。 我们还支持 Windows 10 预览。
 
    [下载](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx)现在的远程应用程序客户端。
- **Azure 远程应用程序是否支持瘦客户机？** 是的支持以下 Windows Embedded 瘦客户端︰
    - Windows 使用 Service Pack 1 嵌入标准 7
    - Windows 嵌入式 POSReady7 
    - Windows 嵌入式瘦的 PC 
    - Windows 嵌入式 8.1 行业

- **哪个版本的 Windows 服务器支持的远程桌面会话主机 (RDSH)。** Windows Server 2012 R2。

##支持和反馈

- **我可以免费玩此服务吗？** 是。 没有将免费试用 30 天。 在试验结束后您可以转换到付费帐户 （它可以在生产环境中使用） 或停止使用该服务。 通过转到[manage.windowsazure.com](http://manage.windowsazure.com)开始免费的试用版-创建远程应用程序的新实例。 使用免费的试用版，您可以与每个实例的 10 个用户构建 RemoteApp 的 2 个。 请记住此试用版仅居住 30 天。
- **什么是远程应用程序的支持计划？** 无偿提供的记帐和订阅管理支持。 通过[Azure 服务计划](../../../support/plans/)提供了技术支持。 此外可以通过我们的[Azure 论坛](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp)免费的社区支持。 
- **多少？ RemoteApp 成本** 签出[Azure RemoteApp 定价详细信息](../../../pricing/details/remoteapp/)。
- **如何提交反馈？** 访问[反馈论坛](http://feedback.azure.com/forums/247748-azure-remoteapp)。
- **若要了解有关 Azure RemoteApp，我该谈谁？** 除了我们的[论坛](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp)，这是提出一个非常好，可以加入每周[专家网络研讨会提出](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html)，我们应讨论远程应用程序的所有事宜。
- **远程应用程序文档呢？** 我们高兴您能提出。 除了门户帮助银箱中的帮助内容 （只需单击**？** 在门户网站中任何网页），在以下的文章可以用来教您所有有关远程应用程序︰
    - **入门︰**
        - [RemoteApp 是什么？](remoteapp-whatis.md)
        - [在远程应用程序模板图像是什么？](remoteapp-images.md)
        - [如何授权呢？](remoteapp-licensing.md)
        - [如何远程应用程序和办公室工作在一起？](remoteapp-o365.md)
        - [如何重定向工作中远程应用程序](remoteapp-redirection.md)是什么？
    - **部署︰**
        - [创建自定义模板映像](remoteapp-create-custom-image.md)
        - [创建混合集合](remoteapp-create-hybrid-deployment.md)
        - [创建云集合](remoteapp-create-cloud-deployment.md)
        - [将 Azure Active Directory 配置为远程应用程序](remoteapp-ad.md)
        - [在远程应用程序中发布应用程序](remoteapp-publish.md)
    - **管理︰**
        - [添加用户](remoteapp-user.md)
        - [配置和使用远程应用程序的最佳做法](remoteapp-bestpractices.md)  

    视频 ！ 我们还提供了大量的视频有关远程应用程序。 某些提供简介 （[Azure RemoteApp 简介](http://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)），而其他人引导您完成部署 （[云部署](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be)和[混合部署](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)）。 签出这些 ！

 