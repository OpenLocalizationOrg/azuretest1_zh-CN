---
ms.openlocfilehash: bf9774dd2214d591d6679ab60d530d30eaa99cf6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="从 Web 应用程序中的 Azure CDN 提供内容" 
    description="教程，教您如何使用 CDN 的内容来提高 Web 应用程序的性能。" 
    services="cdn" 
    documentationCenter=".net" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="cdn" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="cephalin"/>

# 从 Web 应用程序中的 Azure CDN 提供内容 #

本教程展示了如何充分利用 Azure CDN 来提高覆盖范围和 Web 应用程序的性能。 Azure CDN 可以帮助提高 Web 应用程序的性能时︰

- 您的网页上有很多链接到静态或半静态内容
- 全局范围内的客户端访问应用程序
- 您想要减轻来自 Web 服务器的通信
- 您想要减少的并发客户端连接到 Web 服务器 （没有在[Bundling 和缩小](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)很好对此讨论） 
- 您想要增加您的网页的感觉的加载/刷新时间

## 您将了解 ##

在本教程中，您将了解如何执行以下操作︰

-   [提供静态内容从 Azure CDN 终结点](#deploy)
-   [自动化内容上载到 CDN 的终结点的 ASP.NET 应用程序从](#upload)
-   [配置 CDN 缓存以反映所需的内容更新](#update)
-   [提供立即使用查询字符串的最新内容](#query)

## 您将需要 ##

本教程中必须满足以下先决条件︰

-   活动的[Microsoft Azure 帐户](/account/)。 您可以注册试用帐户
-   [Azure SDK](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)为 blob 的管理 GUI 的 Visual Studio 2013 年
-   [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)（使用的[--自动化内容传到 CDN 的终结点的 ASP.NET 应用程序](#upload)）

> [AZURE.NOTE] Azure 帐户来完成本教程，您需要︰
> + 您可以[免费开设 Azure 帐户](/pricing/free-trial/?WT.mc_id=A261C142F)-获取积分可用于试验 Azure 服务付费，并且即使他们习惯之后最多可以保留该帐户并使用释放 Azure 服务，例如网站。
> + 您可以[激活 MSDN 订户权益](/pricing/member-offers/msdn-benefits-details/)-您 MSDN 订阅提供了积分可用于付费的 Azure 服务每月。

<a name="static"></a>
## 提供静态内容从 Azure CDN 终结点 ##

在本教程部分，您将学习如何创建 CDN 并使用它来为静态内容提供服务。 所涉及的主要步骤如下︰

1. 创建存储帐户。
2. 创建链接到存储帐户的 CDN
3. 在您的存储帐户创建 blob 容器
4. 将内容上载到 blob 容器
5. 链接到您上载使用其 CDN URL 的内容

让我们动手吧。 请按照下面的步骤来开始使用 Azure CDN 操作︰

1. 若要创建一个 CDN 的终结点，请登录到[Azure 的管理门户](http://manage.windowsazure.com/)。 
1. 通过单击来创建存储帐户**新建 > 数据服务 > 存储 > 快速创建**。 指定一个 URL，一个位置，然后单击**创建存储帐户**。 

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-1.PNG)

    >[AZURE.NOTE] 请注意，我使用东亚的区域按原样远我测试我从后面北美的 CDN。

2. 一旦新的存储帐户状态为**联机**时，创建新的 CDN 终结点绑定到您创建的存储帐户。 单击**新 > 应用程序服务 > CDN > 快速创建**。 选择创建存储帐户，然后单击**创建**。

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-2.PNG)

    >[AZURE.NOTE] 一旦创建您 CDN，Azure 门户将向您展示其 URL 并绑定到源域。 但是，可能需要一段时间要完全将传播到所有节点位置的 CDN 终结点的配置。  

3. 测试以确保它是在线通过 ping 您 CDN 终结点它。 如果 CDN 端点已不会传播到所有节点中，您将看到类似于下面的消息。

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-3-fail.PNG)

    等待另一个小时，并再次测试。 一旦 CDN 端点已完成传播到这些节点，它应该响应您 ping。

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-3-succeed.PNG)

4. 此时，您已经可以看到 CDN 端点决定位置是给您的最接近 CDN 节点。 从我的台式计算机的响应的 IP 地址是**93.184.215.201**。 将它插入类似[www.ip-address.org](http://www.ip-address.org)的网站并查看服务器位于华盛顿特区

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-4.PNG)

    CDN 节点的所有位置的列表，请参阅[Azure 内容传递网络 (CDN) 节点的位置](http://msdn.microsoft.com/library/azure/gg680302.aspx)。

3. 返回在 Azure 的门户中， **CDN**选项卡中，单击刚创建的 CDN 终结点的名称。

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-2-enablequerya.PNG)

3. 单击**启用查询字符串**可以在 Azure CDN 缓存的查询字符串。 一旦您启用此功能，使用不同的查询字符串访问同一个链路将缓存作为单独的项。

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-2-enablequeryb.PNG)

    >[AZURE.NOTE] 启用查询字符串不是本教程的这一部分的必要条件，而要这样做，因为早期尽可能为便利起见，因为任何更改此处要从容地传播到其他节点，并且您不想要阻塞 （更新 CDN 内容将稍后讨论） 的 CDN 缓存任何未查询字符串-启用的内容。 您将了解如何利用此 — — 在[提供立即通过查询字符串的最新内容](#query)。

6. 在 Visual Studio 2013，在服务器资源管理器中，单击**连接到 Microsoft Azure**按钮。

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-5.PNG)

7.  请按照提示您登录到您的 Azure 帐户。 
8.  一旦您登录，展开**Microsoft Azure > 存储 > 您的存储帐户**。 用鼠标右键单击**Blob** ，选择**创建 Blob 容器**。

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-6.PNG)

8.  指定一个 blob 容器名称并单击**确定**。

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-7.PNG)

9.  在服务器资源管理器中双击 blob 容器创建的用来管理它。 您应该看到在中心窗格中的管理界面。

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-8.PNG)

10. 单击**上载 Blob**按钮来上载图像、 脚本或到 blob 容器所使用的 Web 页的样式表。 上载进度将显示在**Azure 活动日志**中，并上载 blob 将容器视图中显示。 

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-9.PNG)

11. 现在，您已上载的 blob，必须使其公共可访问这些文件。 在服务器资源管理器，右键单击容器，选择**属性**。 **公共的读访问权限**属性设置为**Blob**。

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-10.PNG)

12. 通过导航到浏览器中的 blob 之一的 URL 测试您的 blob 的公共访问。 例如，我可以测试与上载列表中第一个图像`http://cephalinstorage.blob.core.windows.net/cdn/cephas_lin.png`。

    请注意，我没有使用 HTTPS 地址在 Visual Studio 中的 blob 管理界面中。 通过使用 HTTP，测试内容是可公开访问，这是 Azure CDN 的必要条件。

13. 如果您可以看到在您的浏览器中正确呈现该 blob，更改 URL 从`http://<yourStorageAccountName>.blob.core.windows.net`Azure CDN 的 url。 在我种情况下，若要测试的第一幅我 CDN 终结点，将使用`http://az623979.vo.msecnd.net/cdn/cephas_lin.png`。

    >[AZURE.NOTE] 在 Azure 的管理门户，CDN 选项卡中，可以找到 CDN 终结点的 URL。

    比较直接的 blob 存取和 CDN 访问的性能时，您可以看到从使用 Azure CDN 中获得的性能。 下面是映像的我的 blob URL 访问 Internet Explorer 11 F12 开发人员工具屏幕快照︰

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-11-blob.PNG)

    CDN 的 URL 访问相同的图像 

    ![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-static-11-cdn.PNG)

    注意数字**请求**计时，到第一个字节的时间或发送请求和接收来自服务器的第一个响应的时间。 当我访问 blob，承载在东亚地区，它将 266 ms 为我的由于请求必须遍历整个太平洋海只是为了获取对服务器的。 但是，当我访问 Azure CDN，花费仅 16 毫秒，即几乎**20-fold 获得的性能**！
    
15. 现在，则只需在 Web 页中使用新的链接。 例如，可以添加下面的图像标记︰

        <img alt="Mugshot" src="http://az623979.vo.msecnd.net/cdn/cephas_lin.png" />

在本节中，您学习了如何创建 CDN 终结点，将内容上载到它，并将任何 Web 页链接到 CDN contentfrom。

<a name="upload"></a>
## 自动化内容上载到 CDN 的终结点的 ASP.NET 应用程序从 ##

如果您想要轻松地上载的所有静态内容在 ASP.NET Web 应用程序到您 CDN 的终结点，或者如果您部署 Web 应用程序使用不断提供 （例如，见[不断在 Azure 的云服务的提供](../cloud-services/cloud-services-dotnet-continuous-delivery.md)），您可以使用 Azure PowerShell 每次部署 Web 应用程序时自动的 Azure blob 的最新内容文件的同步。 例如，您可以运行脚本在[ASP.NET 应用程序中对 Azure Blob 上载的内容文件](http://gallery.technet.microsoft.com/scriptcenter/Upload-Content-Files-from-41c2142a)上载所需的所有内容文件 ASP.NET 应用程序中。 若要使用此脚本︰

4. 从**开始**菜单运行**Microsoft Azure PowerShell**。
5. 在 Azure PowerShell 窗口中，运行`Get-AzurePublishSettingsFile`下载发布 Azure 帐户设置文件。
6. 一旦您下载您发布的设置文件，请运行以下命令︰ 

        Import-AzurePublishSettingsFile "<yourDownloadedFilePath>"

    >[AZURE.NOTE] 一旦导入您发布的设置文件，它将默认值用于所有 Azure PowerShell 会话的 Azure 帐户。 这意味着，上述步骤只需执行一次。
    
1. 从[下载页面](http://gallery.technet.microsoft.com/scriptcenter/Upload-Content-Files-from-41c2142a)中下载该脚本。 将其保存到您的 ASP.NET 应用程序的项目文件夹。
2. 用鼠标右键单击下载的脚本，然后单击**属性**。
3. 单击**取消阻止**。
4. 打开 PowerShell 窗口，运行下面的命令︰

        cd <ProjectFolder>
        .\UploadContentToAzureBlobs.ps1 -StorageAccount "<yourStorageAccountName>" -StorageContainer "<yourContainerName>"

此脚本将上载到指定的存储帐户和容器*\Content*和*\Scripts*文件夹中的所有文件。 它具有以下优点︰

-   自动复制 Visual Studio 项目的文件结构
-   根据需要自动创建 blob 容器
-   重复使用相同的 Azure 存储帐户和 CDN 端点的多个 Web 应用程序，每个单独的 blob 容器中
-   很容易用新的内容更新 Azure CDN。 更新的内容的详细信息，请参阅[配置 CDN 缓存以反映所需的内容更新](#update)。

对于`-StorageContainer`参数，所以应该使用 Web 应用程序的名称或 Visual Studio 项目名称。 而我以前作为容器名称中使用泛型"cdn"，使用 Web 应用程序的名称允许相关的内容组织到同一个易于识别的容器。

完成后上传的内容，您可以链接到任何在*\Content*和*\Scripts*文件夹在 HTML 代码中，如在.cshtml 文件中，使用`http://<yourCDNName>.vo.msecnd.net/<containerName>`。 下面是一个示例的东西我可以使用 Razor 视图中︰ 

    <img alt="Mugshot" src="http://az623979.vo.msecnd.net/MyMvcApp/Content/cephas_lin.png" />

将 PowerShell 脚本集成到您的连续传递配置示例，请参阅[在 Azure 的云服务的连续传递](../cloud-services/cloud-services-dotnet-continuous-delivery.md)。 

<a name="update"></a>
## 配置 CDN 缓存以反映所需的内容更新 ##

现在，假设之后您上载的静态文件从您的 Web 应用程序中的 blob 容器，您对某一文件作了更改，您的项目中，并再次将其上载到 blob 容器。 您可能会认为它会自动更新到您 CDN 的终结点，但实际上 puzzled，为什么看不到更新反映内容的 CDN URL 的访问时。 

事实是，CDN 会确实会自动更新 blob 存储，但它是通过将默认 7 天缓存规则应用于内容。 这意味着，一旦 CDN 节点提取从 blob 存储内容，相同的内容才会被刷新高速缓存中已过期。

好消息是，您可以自定义缓存过期。 与大多数浏览器都相似，Azure CDN 考虑内容的缓存控制标头中指定的过期时间。 您可以通过导航到 Azure 门户中的 blob 容器并编辑 blob 属性指定自定义的缓存控制标头值。 下面的屏幕快照演示缓存过期时间设置为 1 小时 （3600 秒）。 

![](media/cdn-serve-content-from-cdn-in-your-web-application/cdn-updates-1.PNG)

您还可以执行此 PowerShell 脚本来设置所有 blob 缓存控制标头中。 [--自动化内容传到 CDN 的终结点的 ASP.NET 应用程序](#upload)中的脚本，发现下面的代码段︰

    Set-AzureStorageBlobContent `
        -Container $StorageContainer `
        -Context $context `
        -File $file.FullName `
        -Blob $blobFileName `
        -Properties @{ContentType=$contentType} `
        -Force

并对其进行修改，如下所示︰  

    Set-AzureStorageBlobContent `
        -Container $StorageContainer `
        -Context $context `
        -File $file.FullName `
        -Blob $blobFileName `
        -Properties @{ContentType=$contentType; CacheControl="public, max-age=3600"} `
        -Force

可能仍然需要在过期之前它将新的内容，与新的缓存控制标头您 Azure CDN 上等待 7 天完全缓存的内容。 这说明了一个事实，即如果您希望您的内容的更新，例如 JavaScript 立即投入或 CSS 更新自定义的缓存值没有帮助。 但是，您可以变通解决此问题的 renamiving 文件或版本控制它们通过查询字符串。 有关详细信息，请参阅[提供立即使用查询字符串的最新内容](#query)。

没有，当然，时间和缓存的位置。 例如，您可能不需要频繁的更新，如即将到来的世界杯游戏可以刷新每隔 3 个小时，但获取您想要从 Web 服务器中卸载它足够全球通信的内容。 这可能是一个很好的例子，使用 Azure CDN 缓存。

<a name="query"></a>
## 提供立即使用查询字符串的最新内容 ##

在 Azure CDN，以便与特定查询字符串的 Url 从内容缓存分别可以启用查询字符串。 这是一项强大功能使用如果您想要将某些内容更新推送到客户端浏览器，而不是等待 CDN 缓存内容过期。 假设我将我的 Web 页发布使用 URL 中的版本号。
  
    <link href="http://az623979.vo.msecnd.net/MyMvcApp/Content/bootstrap.css?v=3.0.0" rel="stylesheet"/>

当我发布一个 CSS 更新，我 CSS URL 中使用的不同的版本号︰  

    <link href="http://az623979.vo.msecnd.net/MyMvcApp/Content/bootstrap.css?v=3.1.1" rel="stylesheet"/>

已启用了查询字符串的 CDN 终结点，两个 Url 都是唯一的然后它将对我的 Web 服务器中检索新的*bootstrap.css*进行新请求。 到没有启用了查询字符串的 CDN 终结点，这些都是相同的 URL，但它只是将服务缓存的*bootstrap.css*。 

这一轮然后是自动更新的版本号。 在 Visual Studio，这是容易做到的。 在.cshtml 文件中，我更愿意使用上面的链接，我可以指定基于程序集的版本号。  

    @{
        var cdnVersion = System.Reflection.Assembly.GetAssembly(
            typeof(MyMvcApp.Controllers.HomeController))
            .GetName().Version.ToString();
    }
    
    ...
    
    <link href="http://az623979.vo.msecnd.net/MyMvcApp/Content/bootstrap.css?v=@cdnVersion" rel="stylesheet"/>

如果您更改部件号作为每个发布周期的一部分，然后您同样可以确保获得一个唯一的版本号，每次您发布您的 Web 应用程序，它将保持不变直到下一个发布周期。 也可以使 Visual Studio 自动递增每次通过在 Visual Studio 项目中打开*Properties\AssemblyInfo.cs*生成 Web 应用程序的程序集的版本号，并使用`*`在`AssemblyVersion`。 例如︰

    [assembly: AssemblyVersion("1.0.0.*")]

## 捆绑的脚本和样式表在 ASP.NET 中的呢？ ##

使用[Azure 应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)和[Azure 云服务](/services/cloud-services/)，可以与[ASP.NET 捆绑和缩小](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)的最佳 Azure CDN 集成。 

集成和 Azure CDN 的 Azure 应用程序服务或 Azure 云服务为您提供以下优点︰

- 作为 Azure 的 web 应用程序的[连续部署](../web-sites-publish-source-control.md)过程的一部分集成内容部署 （图像、 脚本和样式表）
- 方便地升级提供 CDN 服务 NuGet 程序包，如 jQuery 或引导数据库版本 
- 从相同的 Visual Studio 的界面管理您的 Web 应用程序和您提供 CDN 服务的内容

相关的教程，请参阅︰
- [在 Azure 应用程序服务使用 Azure CDN](../cdn-websites-with-cdn.md)
- [与 Azure CDN 集成云服务](cdn-cloud-service-with-cdn.md)

如果没有使用 Azure 应用程序服务 Web 应用程序或 Azure 云服务的集成，就可以为您的脚本包，同时出现下列警告使用 Azure CDN:

- 必须手动将捆绑的脚本上载到 blob 存储。 在[stackoverflow](http://stackoverflow.com/a/13736433)，建议采用编程的解决方案。
- 在.cshtml 文件中，将呈现的脚本 CSS 标记使用 Azure CDN。 例如︰

        @Html.Raw(Styles.Render("~/Content/css").ToString().Insert(0, "http://<yourCDNName>.vo.msecnd.net"))

## 详细信息 ##
- [Azure 内容传递网络 (CDN) 的概述](cdn-overview.md)
- [在 Azure 应用程序服务使用 Azure CDN](../cdn-websites-with-cdn.md)
- [与 Azure CDN 集成云服务](cdn-cloud-service-with-cdn.md)
- [如何将内容传递网络 (CDN) 内容映射到自定义的域](http://msdn.microsoft.com/library/azure/gg680307.aspx)
- [Azure 使用 CDN](cdn-how-to-use-cdn.md)
 

测试
