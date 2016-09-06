---
ms.openlocfilehash: c5b282f44e1f2df1f708e24c4408a007dc80cadd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Windows 应用程序的通用 SDK 升级过程" 
    description="Windows Azure 的移动服务的通用应用程序 SDK 升级过程"                     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/10/2015" 
    ms.author="piyushjo" />

#Windows 应用程序的通用 SDK 升级过程

如果已经有到应用程序中集成服务的较早版本，您必须升级 SDK 时要考虑以下几点。

您可能需要执行几个步骤，如果错过了几个版本的 SDK。 例如，如果迁移从 0.10.1 到 0.11.0，您必须首先按照"从 0.10.1 到 0.9.0"步骤再"发件人为 0.11.0 0.10.1"过程。

##从 2.0.0 为 3.0.0

### 资源
此步骤涉及自定义的资源。 如果您已自定义资源提供的 SDK （html、 图像、 覆盖） 然后您必须在升级之前备份它们并重新应用您的自定义升级资源上。

##1.1.1 与 2.0.0 从

下面介绍如何从 Azure 移动合作由应用程序提供的 Capptain SA Capptain 服务迁移 SDK 集成。 

> [Azure.IMPORTANT] Capptain 和移动签约不是相同的服务，下面给出的过程只重点介绍如何将客户端应用程序的迁移。 迁移的 SDK 应用程序中不会迁移数据从 Capptain 服务器到移动服务服务器

如果要从早期版本迁移，请参考 Capptain 网站首先迁移到 1.1.1，则应用下面的过程

### Nuget 程序包

由**MicrosoftAzure.MobileEngagement** Nuget 程序包替换**Capptain.WindowsPhone** 。

### 应用移动服务

SDK 使用术语`Engagement`。 您需要更新您的项目以匹配此更改。

您需要卸载当前 Capptain nuget 程序包。 请考虑您在 Capptain 资源文件夹中的所有更改将被都删除。 如果您想要保留这些文件，然后复印一份。

之后，您的项目中安装新的 Microsoft Azure 接洽 nuget 程序包。 您可以直接在 [nuget 网站] 上找到它。 或者这里的索引。 此操作将替换所有服务所使用的资源文件并将新服务 DLL 添加到您的项目引用。

您必须清除删除 Capptain DLL 的引用的项目引用。 如果不将此，Capptain 的版本会产生冲突，错误会发生这种情况。

如果您有自定义 Capptain 资源，将复制旧的文件内容并将其粘贴在新的合作文件。 请注意 xaml 和 cs 文件需要更新。

完成这些步骤后您只需替换旧的 Capptain 引用新的服务引用。

1. 所有 Capptain 命名空间都必须进行更新。

    之前迁移︰
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    后迁移︰
    
        using Microsoft.Azure.Engagement;

2. 所有包含"Capptain"的 Capptain 类应包含"协定"。

    之前迁移︰
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    后迁移︰
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. 对于 xaml 文件 Capptain 命名空间和属性也将更改。

    之前迁移︰
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    后迁移︰
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. 叠加页更改
    > [AZURE.IMPORTANT] 此外可以更改覆盖。 其新的命名空间为`Microsoft.Azure.Engagement.Overlay`。 它必须用于 xaml 和 cs 文件。 此外`CapptainGrid`是被命名为`EngagementGrid`， `capptain_notification_content` ，`capptain_announcement_content`被命名为`engagement_notification_content`和`engagement_announcement_content`。
    
    覆盖︰
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    它将成为︰
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. 其他资源，如 Capptain 图片和 HTML 文件，请注意，它们也已重命名使用"合作"。

### 项目声明

在 Package.appxmanifest`File Type Associations`已经更新︰

 -   capptain\_到达\_合约的内容\_到达\_内容
 -   capptain\_日志\_合约的文件\_日志\_文件

### 应用程序 ID / SDK 键

服务使用的连接字符串。 您不必使用移动服务指定应用程序 ID 和 SDK 键，只需要指定一个连接字符串。 您可以设置它对您的 EngagementConfiguration 文件。

可以在中设置服务配置您`Resources\EngagementConfiguration.xml`项目的文件。

编辑此文件，以指定︰

-   应用程序连接字符串标记之间`<connectionString>`， `<\connectionString>`。

如果您想要指定它在运行时，可以调用下面的方法在签约代理初始化之前︰

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

在 Azure 管理门户网站上显示您的应用程序的连接字符串。

### 项目名称更改

名为*capptain*的所有项目已都命名*项目*。 同样为到*签约* *Capptain* 。

常用的 Capptain 项的示例︰

-   现在名为 EngagementConfiguration 的 CapptainConfiguration
-   现在名为 EngagementAgent 的 CapptainAgent
-   现在名为 EngagementReach 的 CapptainReach
-   现在名为 EngagementHttpConfig 的 CapptainHttpConfig
-   现在名为 GetEngagementPageName 的 GetCapptainPageName

请注意，重命名也影响重写方法。

 
测试
