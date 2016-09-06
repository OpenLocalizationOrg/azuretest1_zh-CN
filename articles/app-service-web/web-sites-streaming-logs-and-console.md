---
ms.openlocfilehash: ad495992cd732b17168493afa223c4de29dce435
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="流式传输日志和控制台" 
    description="流式传输日志和控制台概述" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/10/2015" 
    ms.author="byvinyal"/>

#流式传输日志和控制台

### 流式处理日志 ###

Microsoft Azure 门户网站提供了用于实时查看跟踪事件从 Azure 应用程序服务 web 应用程序集成流日志查看器。  

此设置需要几个简单步骤︰

- 在代码中编写跟踪
- 启用应用程序诊断从 Azure 门户中
- 单击 web 应用程序的刀片式服务器上流式的日志部分

### 如何在代码中编写跟踪 ###

编写跟踪在您的代码非常简单。  在 C# 的本息编写以下代码︰

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Trace 类如何命名空间中的生存之地。

Node.js 应用程序中可以编写此代码，以获得相同的结果︰

`````````````````````````
console.log("My trace statement").
`````````````````````````

### 如何启用和查看流记录 ###

![][BrowseSitesScreenshot]
诊断程序可在每个 web 应用程序的基础。  从[门户](https://portal.azure.com)中单击 **浏览 (1)**左边的菜单栏上的按钮，然后单击**Web 应用程序 (2)**到**(3) 列出**的所有 web 应用程序。  

单击您想要配置以导航到 web 应用程序的名称。
  
![][DiagnosticsLogs]
然后单击**设置 (1)** > **诊断日志 (2)** ，并**打开** 
**应用程序记录 (Filesystem)(3)** **级别**选项 letts 您更改的跟踪，以捕获的严重级别。  如果您只想要逐渐熟悉功能，因为这将确保所有跟踪语句获取记录，应设置**详细**到这。

单击**保存**顶部的刀片式服务器，您就可以查看日志。

**注意︰**更高**严重性级别**日志和详细跟踪消耗更多的资源将获得。 请确保将其设置为适当的级别高的通信中使用此功能时 / 生产站点。 

![][StreamingLogsScreenshot]
若要查看这种流从门户中的日志，请单击**工具 (1)** > **Stream(2) 日志**。 如果您的应用程序正在写入跟踪语句然后您应该看到它们在接近实时结果窗口中。

## 控制台 ##

Azure 门户提供控制台访问您的 web 应用程序环境。 您可以浏览您的 web 应用程序的文件系统和运行 powershell/cmd 脚本。  是由相同的权限将设置为运行 web 应用程序代码执行控制台命令时进行绑定。 您不能访问受保护的目录或运行需要提升的权限的脚本。  

![][ConsoleScreenshot]
要获取到控制台，请浏览到 web 应用程序上一节中所述。 单击**工具**>将打开**控制台**，该控制台。

若要获取熟悉控制台，请尝试诸如此类的基本命令︰



`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````



<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png

测试
