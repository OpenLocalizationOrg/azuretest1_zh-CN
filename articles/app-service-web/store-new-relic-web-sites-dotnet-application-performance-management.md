---
ms.openlocfilehash: 5df00577b44d8ab4d2745d1d5caff21d1e81ac86
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle=".NET web 应用程序在 Azure 应用程序服务使用新的 Relic 应用程序性能管理" 
    description="了解如何使用新的 Relic 性能监视 Azure 应用程序服务上运行的 ASP.NET 应用程序。" 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/30/2015" 
    ms.author="stepsic"/>



# .NET web 应用程序在 Azure 应用程序服务使用新的 Relic 应用程序性能管理

本指南介绍了如何添加新的 Relic 世界一流性能监视到[Azure 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)服务 web 应用程序。 我们将介绍将新 Relic 添加到您的应用程序，并向您介绍了一些新的 Relic 功能的快速而简单过程。 有关使用新的 Relic 的详细信息，请参阅[使用新 Relic](#using-new-relic)。

## 新 Relic 是什么？

新的 Relic 是一个专注于开发人员工具，它监视您的生产应用程序，并提供深入了解它们的性能和可靠性。 它旨在节约您的时间识别和诊断性能问题时，它将需要解决这些问题，触手可及的信息。

新的 Relic 跟踪加载时间和吞吐量 web 事务，同时从服务器和用户的浏览器。 它显示在数据库中，您花费多少时间分析慢速查询和 web 请求时，提供了正常运行时间监视和警报、 跟踪应用程序异常和一大笔更多。

## 新 Relic 特殊定价通过 Azure 存储

新 Relic 标准都是免费的 Azure 用户。
新 Relic 专业提供基于多个包中的网站模式所使用的而且如果您使用的实例的大小保留模式。

价格信息请参阅[Azure 市场上的新 Relic 页](/marketplace/partners/newrelic/newrelic)。

> [AZURE.NOTE] 定价将只列出最多 10 个计算实例。 计数大于 10 请联系新的 Relic (sales@newrelic.com) 的批发价。

Azure 客户获得新 Relic 专业 2 周试用的订阅他们部署新的 Relic 代理。

注册了一个新的 Relic 使用 Azure 市场
--

新的 Relic 与 Azure Web 角色，辅助角色和 Azure 应用程序服务无缝集成。

若要注册新 Relic 直接从 Azure 市场，按照以下四个简单的步骤。

## 第 1 步。 创建一个新的 Relic 帐户

1. 登录到[Azure 预览门户](https://portal.azure.com)和角中单击**新建**。
3. 单击**开发人员服务** > **Relic APM 新**。
4. 将新的 Relic 帐户配置通过指定以下内容，然后单击**创建**。
    - **名称**
    - **定价层**
    - **资源组**
    - **订阅**
    - **位置**
    - **法律条款**

11. 单击**创建**后，新的 Relic 帐户将开始创建过程。 通过单击**通知**按钮，您可以监视的状态。 一旦创建，将显示新的 Relic 帐户的刀片式服务器。

12. 若要检索您的新的 Relic 许可证密钥，请看**精要**面板顶部的刀片式服务器。 集成您的 web 应用程序与新的 Relic 帐户时，您的 Web 应用程序实例自动将其应用程序设置中注册此许可证密钥。

## 步骤 2︰ 配置您的 web 应用程序的新 Relic 集成

1. 在[Azure 预览门户](https://portal.azure.com)打开 web 应用程序的刀片式服务器。
2. 单击顶部的刀片式服务器的"..."菜单并选择**添加平铺**。
3. 在**监视**选项卡上选择**应用程序摘要**，并将其拖动到所需的拼贴显示在 web 应用程序的刀片式服务器上的位置。
4. 单击完成以完成添加平铺。
5. 单击**监视应用程序**图块，然后选择**新的 Relic**。
6. 选择您在前面步骤中创建的帐户，然后单击**确定**。 

    ![](./media/store-new-relic-web-sites-dotnet-application-performance-management/configure-new-relic-integration.png)

    一次保存操作完成后，单击 web 应用程序的刀片中的**所有设置**，然后都单击**应用程序设置**。 您应该会看到**NEWRELIC\_有**设置添加到刀片式服务器的**应用程序设置**部分，以支持新的 Relic:

    >[AZURE.NOTE] 它可能需要最多 30 秒让新的应用程序设置生效。 若要强制设置立即生效，请重新启动 web 应用程序。

## 步骤 3︰ 将 ASP.NET web 应用程序发布

使用 Visual Studio，发布您的 web 应用程序。 如果您以前就已经发布您的 web 应用程序，重新发布，以便添加所需的新 Relic NuGet 程序包以启用新的 Relic 监视 Web 应用程序实例。

## 第 4 步。 签出在新的 Relic 应用程序的性能。

若要查看您的新 Relic 仪表板︰

2. 在[Azure 预览门户](https://portal.azure.com)打开 web 应用程序的刀片式服务器。
3. 单击**监视应用程序** > **应用程序名称** > **在新的 Relic 视图**。

    ![](./media/store-new-relic-web-sites-dotnet-application-performance-management/view-new-relic-data.png)

3. 如果这是首次使用您的帐户，配置您的帐户信息。
3. 在新的 Relic 菜单栏中，选择**应用程序 > （应用程序的名称）**。

    **监视 > 概述**仪表板会自动出现。

    ![新 Relic 监测仪表板](./media/store-new-relic-web-sites-dotnet-application-performance-management/NewRelic_app.png)

    您从列表中选择一个应用程序在您**的应用程序**菜单上后，**概述**仪表板将显示当前的应用程序服务器和浏览器信息。

### <a id="using-new-relic"></a>使用新的 Relic

从应用程序菜单上的列表中选择您的应用程序后，概述仪表板将显示当前的应用程序服务器和浏览器信息。 若要在两个视图之间进行切换，请单击**应用程序服务器**或**浏览器**按钮。

除了<a href="https://newrelic.com/docs/site/the-new-relic-ui#functions">标准新 Relic 用户界面</a>和<a href="https://newrelic.com/docs/site/the-new-relic-ui#drilldown">仪表板下钻</a>函数，应用程序概述仪表板具有其他功能。

<table border="1">
  <thead>
    <tr>
      <th><b>如果您希望...</b></th>
      <th><b>执行此操作。.</b></th>
    </tr>
  </thead>
  <tbody>
    <tr>
       <td>显示所选应用程序 & #39; s 服务器或浏览器的仪表板信息</td>
       <td>单击<b>应用程序服务器</b>或<b>浏览器</b>按钮。</td>
    </tr>
     <tr>
       <td>查看阈值级别，您的应用程序 & #39; s <a href="https://newrelic.com/docs/site/apdex" target="_blank">Apdex</a>分数</td>
       <td>Apdex 分数指向<b>？<b> 图标。</b></b></td>
    </tr>
    <tr>
       <td>查看全球 Apdex 详细信息</td>
       <td>从概述 & #39; s<b>浏览器</b>视图中，全局 Apdex 图上任意点。<br /><b>提示︰</b>直接跳转到所选应用程序 & #39; s<a href="https://newrelic.com/docs/site/geography" target="_blank">地理</a>仪表板，请单击<b>全局 Apdex</b>的标题，或全局 Apdex 图上的任意位置单击。</td>
    </tr>
    <tr>
       <td>查看<a href="https://docs.newrelic.com/docs/applications-menu/transactions-dashboard" target="_blank">Web 事务</a>仪表板</td>
       <td>单击应用程序概述仪表板上的 Web 事务表。 或者，若要查看有关特定 web 交易记录的详细信息 (包括<a href="https://newrelic.com/docs/site/key-transactions" target="_blank">键事务</a>)，请单击其名称。</td>
    </tr>
    <tr>
       <td>查看<a href="https://newrelic.com/docs/site/errors" target="_blank">错误</a>仪表板</td>
       <td>单击错误率图表 & #39; s 应用程序概述仪表板上的标题。<br /><b>提示︰</b>您还可以查看从错误面板<b>应用程序</b> &gt; （您的应用程序）&gt;事件&gt;错误。</td>
    </tr>
    <tr>
       <td>查看应用程序 & #39; s 服务器详细信息</td>
       <td><p>执行下列任一操作︰<p>
        <ul>
          <li>主机表视图或专题会议指标详细信息的每个主机之间进行切换。</li>
          <li>单击单个服务器 & #39; s 名称。</li>
          <li>指向单个服务器 & #39; s Apdex 分数。</li>
          <li>单击单个服务器 & #39; s CPU 使用情况或内存。</li>
        </ul>
       </p></p></td>
    </tr>
  </tbody>
</table>

下面是一个示例应用程序概述仪表板选择浏览器视图时。

![程序包管理器控制台](./media/store-new-relic-web-sites-dotnet-application-performance-management/NewRelic_app_browser.png)

## 下一步行动

请查看以下这些附加资源的详细信息︰

 * [安装.NET 代理的 Azure 网站](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-websites#manual)︰ 新 Relic.NET 代理安装过程 
 * [新的 Relic 用户界面](https://newrelic.com/docs/site/the-new-relic-ui)︰ 概述新 Relic 的用户界面，设置用户权限和配置文件，使用标准函数和仪表板深入查看详细信息
 * [应用程序概述](https://newrelic.com/docs/site/applications-overview)︰ 特性和功能时使用新的 Relic 应用程序概述仪表板
 * [Apdex](https://newrelic.com/docs/site/apdex)︰ 概述如何 Apdex 测量与应用程序的最终用户的满意度
 * [实际用户监视](https://newrelic.com/docs/features/real-user-monitoring)︰ RUM 详细时间的说明概述所需用户的浏览器加载网页，它们来自何处，以及他们使用何种浏览器
 * [查找帮助](https://newrelic.com/docs/site/finding-help)︰ 通过新 Relic 的在线帮助中心可用资源

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* Azure 门户到 Azure 预览门户的更改的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)


[vswebsite]: web-sites-dotnet-get-started.md

[wmnugetbutton]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nrwmnugetbutton.png
[wmnugetgallery]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nrwmnugetgallery.png

[newrelicconf]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nrwmlicensekey.png
[vslicensekey]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nrvslicensekey.png
[加载项]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nraddon.png
[自定义]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nrcustom.png
 
测试
