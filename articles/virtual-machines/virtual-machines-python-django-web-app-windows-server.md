---
ms.openlocfilehash: 18b4153c1c1412744914781afd0f654efadbf763
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Python 的 web 与 Django 应用程序 |Microsoft Azure"
    description="教程，教您如何承载基于 Django 的网站在 Azure 上使用 Windows Server 2012 R2 数据中心的虚拟机。"
    services="virtual-machines"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""/>


<tags 
    ms.service="virtual-machines" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>




# Django Hello World web 应用程序

<div class="dev-center-tutorial-selector sublanding"><a href="/develop/python/tutorials/web-app-with-django/" title="Windows" class="current">Windows</a><a href="/develop/python/tutorials/django-hello-world-(maclinux)/" title="MacLinux">Mac/Linux</a></div>

本教程介绍如何承载基于 Django 的网站在 Microsoft Azure 使用 Windows 服务器的虚拟机上。 本教程假定您已经使用 Azure 没有经验。 在完成本教程之后, 必须组成一个基于 Django 应用程序并在云中运行。

您将学习如何︰

* 设置主机 Django Azure 的虚拟机。 虽然本教程说明如何做到这一点在 Windows 服务器下，相同还能与 Linux 在 Azure 中承载的虚拟机。
* 从 Windows 创建一个新的 Django 应用程序。

通过遵循本教程中，您将生成一个简单的 Hello World web 应用程序。 在 Azure 的虚拟机将承载的应用程序。

已完成应用程序的屏幕快照显示下一步。

![在 Azure 上显示你好世界页上的浏览器窗口][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## 创建和配置到主机 Django Azure 的虚拟机

1. 按照给定的[此处](virtual-machines-windows-tutorial-classic-portal.md)创建 Windows Server 2012 R2 数据中心分布的 Azure 的虚拟机。

1. 指示 Azure 来引导端口 80 的通信从网站到虚拟机上的端口 80:
 - 导航到 Azure 门户中您新创建的虚拟机并单击**终结点**选项卡。
 - 单击屏幕底部的**添加**按钮。
    ![添加终结点](./media/virtual-machines-python-django-web-app-windows-server/django-helloworld-addendpoint.png)

 - 打开**TCP**协议作为**专用端口 80**的**公用端口 80** 。
![][port80]
1. 从**仪表板**选项卡，单击**连接**使用**远程桌面**来远程登录到新创建的 Azure 虚拟机。  

**的重要说明︰**所有下面的说明假定正常登录到虚拟机并发布命令存在，而不是您的本地计算机。

## <a id="setup"> </a>安装 Python，Django，WFastCGI

**注意︰**要下载使用您可能需要配置 IE esc 键设置的 Internet Explorer (开始/管理工具/服务器管理器/本地服务器，然后单击**IE 增强的安全配置**、 设置为 Off)。

1. 从[python.org][]中安装最新的 Python 2.7 或 3.4。
1. 安装使用 pip 的 wfastcgi 和 django 软件包。

    对于 Python 2.7，请使用下面的命令。

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    对于 Python 3.4，使用下面的命令。

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## 与 FastCGI 安装 IIS。

1. FastCGI 支持安装 IIS。  这可能需要几分钟时间来执行。

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## 创建新的 Django 应用程序

1.  从*C:\inetpub\wwwroot*，输入以下命令以创建一个新的 Django 项目︰

    对于 Python 2.7，请使用下面的命令。

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    对于 Python 3.4，使用下面的命令。

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![新的 AzureService 命令的结果](./media/virtual-machines-python-django-web-app-windows-server/django-helloworld-cmd-new-azure-service.png)

1.  **Django 管理**命令生成基于 Django 的网站的基本结构︰

  -   **helloworld\manage.py**可以帮助您启动承载和停止承载基于 Django 的网站
  -   **helloworld\helloworld\settings.py**包含 Django 应用程序设置。
  -   **helloworld\helloworld\urls.py**包含每个 url 及其视图之间的映射代码。

1.  创建一个名为**views.py**的*C:\inetpub\wwwroot\helloworld\helloworld*目录中的新文件。 这将包含呈现"你好世界"页视图。 启动您的编辑器，输入以下命令︰

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Urls.py 文件的内容替换为以下。

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## 将 IIS 配置

1. 解除锁定全局 applicationhost.config 的处理程序部分。  这将允许使用 web.config 中的 python 处理。

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. 启用 WFastCGI。  这将添加应用程序可以引用您的 Python 解释器可执行文件和 wfastcgi.py 脚本全局 applicationhost.config。

    2.7 Python:

        c:\python27\scripts\wfastcgi-enable

    3.4 Python:

        c:\python34\scripts\wfastcgi-enable

1. 在*C:\inetpub\wwwroot\helloworld*中创建一个 web.config 文件。  值`scriptProcessor`属性应与执行上个步骤的输出相匹配。  请参阅更多的 wfastcgi 设置的 pypi 上的[wfastcgi][]页。

    2.7 Python:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    3.4 Python:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. 更新的 IIS 默认网站指向 django 项目文件夹的位置。

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. 最后，加载 web 页在浏览器中。

![在 Azure 上显示你好世界页上的浏览器窗口][1]


## 关闭您 Azure 的虚拟机

完成本教程后，关闭，和/或删除您新创建 Azure 的虚拟机，为其他教程释放资源并避免会导致 Azure 的使用费。

[1]: ./media/virtual-machines-python-django-web-app-windows-server/django-helloworld-browser-azure.png

[端口 80]: ./media/virtual-machines-python-django-web-app-windows-server/django-helloworld-port80.png

[Web 平台安装程序]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
