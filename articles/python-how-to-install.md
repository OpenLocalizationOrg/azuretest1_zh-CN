---
ms.openlocfilehash: 0be7ed8b1878e7b8c094d3a18b8f06bb5cb3d8cc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Python 和 SDK-Azure 安装"
    description="了解如何安装使用 Azure 的 Python 和 SDK。"
    services=""
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="08/31/2015"
    ms.author="huvalo"/>

# Python 和 SDK 安装

Python 是很容易的安装在 Windows 和 Mac 和 Linux 上之前，预安装。  本指南将引导您完成安装并准备好您的计算机使用与 Azure。

## 在 Python 中什么是 Azure SDK？

Python 的 Azure SDK 包含允许您开发、 部署和管理 Azure 的 Python 应用程序的组件。 具体来说，Python 的 Azure SDK 包含下列内容︰

* **Azure Python 客户端库**。 这些类库提供的接口用于访问 Azure 的功能，例如存储和服务总线和管理 Azure 的资源，如存储帐户、 虚拟机等。
* **Azure 仿真程序 (仅限 Windows)**。 计算和存储的仿真程序是本地的云服务和数据管理服务，您可以测试应用程序本地的仿真程序。 仅在 Windows Azure 仿真程序运行。

## Python 和要使用的版本

有几种不同特色的 Python 解释器-示例包括︰

* CPython 的标准和最常使用 Python 解释器
* IronPython-在.Net/CLR 运行的 Python 解释器
* Jython 的 Python 解释器在 JVM 上运行

仅**CPython**经过测试并且 Python Azure SDK 和 Azure 的服务，例如网站和云服务支持。  我们建议 2.7 或 3.4 版。

## 从何处获取 Python？

有几种方法来获取 CPython:

* 直接从[www.python.org][]
* 从[www.continuum.io][]、 [www.enthought.com][]或[www.activestate.com][]等知名 distro
* 从源代码生成 ！

除非您有特定的需要，我们建议前两个选项，如下所述。

## 安装 Windows、 Linux 和 MacOS （仅适用于客户端库）

如果您已经有了 Python 安装，可以使用 pip 现有 Python 2.7 或 Python 3.3 + 环境中安装的所有客户端库的包。 这将从[Python 包索引][](PyPI) 下载的程序包。

请注意，您可能需要使用`sudo`ie 上 Linux 和 MacOS 的命令。`sudo pip install azure`.

    pip install azure

从 1.0.0 版开始，库已拆分为多个文件包。 现在，您可以安装软件包所需或使用捆绑包。

若要安装 Azure 存储运行时客户端库︰

    pip install azure-storage

若要安装 Azure 服务总线运行时客户端库︰

    pip install azure-servicebus

若要安装 Azure 资源管理器 (ARM) 客户端库︰

    pip install azure-mgmt

若要安装 Azure 服务管理 (ASM) 客户端库︰

    pip install azure-servicemanagement-legacy


## 安装在 Windows （Python，Azure 仿真程序和客户端库）

Web 平台安装程序可用于简化安装。 其中包括 CPython 从[www.python.org][]。

* [Python 2.7 的 Microsoft Azure SDK][]
* [Python 3.4 的 Microsoft Azure SDK][]

**注意︰**在 Windows 服务器上，以便下载 WebPI 安装程序您可能需要配置 IE esc 键设置 (开始/管理工具/服务器管理器/本地服务器，然后单击**IE 增强的安全配置**、 设置为 Off)

### Python 2.7

WebPI 安装程序提供了开发 Python Azure 应用程序所需要的一切。

![how-to-install-python-webpi-27-1](./media/python-how-to-install/how-to-install-python-webpi-27-1.png)

安装完毕后，请键入`python`在提示符下以确保一切过程顺利进行。  这取决于您的安装，您可能需要设置路径变量来查找 （的正确版本） Python:

![how-to-install-python-win-run-27](./media/python-how-to-install/how-to-install-python-win-run-27.png)

在安装后应该具备 Python 和客户端库提供的默认位置︰

        C:\Python27\Lib\site-packages\azure


### Python 3.4

WebPI 安装程序提供了开发 Python Azure 应用程序所需要的一切。

![how-to-install-python-webpi-34-1](./media/python-how-to-install/how-to-install-python-webpi-34-1.png)

安装完成后，在提示符下以确保所有步骤类型 python 过程顺利进行。  这取决于您的安装，您可能需要设置路径变量来查找 （的正确版本） Python:

![how-to-install-python-win-run-34](./media/python-how-to-install/how-to-install-python-win-run-34.png)

在安装后应该具备 Python 和客户端库提供的默认位置︰

        C:\Python34\Lib\site-packages\azure

### Windows 卸载

**Python 的 Azure SDK** WebPI 产品不是在典型的意义，但实际的独特产品如 32 位 Python 2.7/3.4，Azure 的客户端库的 Python 等捆绑在一起的集合的应用程序。  这样的后果是它有自己的没有传统的卸载程序，因此您将需要删除分别从控制面板安装这些程序。  

如果要重新安装**Python 的 Azure SDK**，只需打开 PowerShell 命令提示符以管理员的身份，并运行下面的命令︰

    rm -force "HKLM:\SOFTWARE\Microsoft\Python Tools for Azure"

然后重新运行 WebPI。

## 获取更多软件包

[Python 包索引][](PyPI) 具有选择丰富的 Python 库。  如果您选择安装 Distro，将已经大部分 web 开发方案为技术计算各种有趣。


## Visual Studio 的 Python 工具

[Visual Studio 的 Python 工具][](PTVS) 是来自 Microsoft 的 VS 变成成熟的 Python IDE 自由操作系统插件︰

![how-到-安装-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

使用 PTVS 是可选的但建议使用，因为它为您提供 Python 和 Web 项目/解决方案支持、 调试、 性能分析、 交互窗口、 模板编辑和智能感知。

PTVS 还便于部署到 Microsoft Azure 支持部署到[云服务][]和[网站][]。

PTVS 与您现有的 Visual Studio 2013年或 2015年安装工作。  文档、 下载和讨论，请参阅[Visual Studio 的 Python 工具]。  

## Python 的 Linux 和 MacOS Azure 方案

对于 Linux 或 MacOS，这些是主要的 Azure 方案支持︰

1. 通过对 Python 中使用客户端库使用 Azure 服务

2. 在 Linux 虚拟机中运行您的应用程序

3. 开发和使用 Git 的 Azure 网站发布

第一个方案中，您可以创作丰富的 web 应用程序利用 Azure REST API 的 Azure PaaS 的功能，如[blob 存储][][队列存储][]、[表存储][]通过 Pythonic 包装等。  这些在 Windows、 Mac 和 Linux 工作方式都相同。  您还可以从本地开发计算机或一个 Azure 上运行的 Linux 虚拟机使用这些客户端库。

对于虚拟机方案中，您只需启动 Linux 虚拟机 （Ubuntu，CentOS 的 Suse） 所选择的和管理运行/您所喜欢。  举一个例子，可以在 Windows/Mac/Linux 机器上运行[IPython][]的 REPL 笔记本和您浏览器指向 Linux 或 Windows Azure 上运行 IPython 引擎的多处理器虚拟机。 [在 Azure 上 IPython 笔记本][]教程的详细信息，请参阅。

有关如何安装 Linux 虚拟机的信息，请参阅[创建虚拟机运行 Linux][]教程。

使用 Git 部署，可以开发一个 Python web 应用程序并将其发布到 Azure 网站从任何操作系统。  当推向 Azure 存储库时，它将自动创建一个虚拟环境，pip 安装您所需的程序包。

开发和发布 Azure 网站的详细信息，请参见[创建具有 Django 的网站][]、[瓶子与创建网站][]和[Flask 与创建网站][]的教程。 在使用任何 WSGI 符合框架的详细信息，请参阅[与 Azure 网站配置 Python][]。


## 其他软件和资源︰

* [生命周期分析 Python 分发][]
* [Enthought Python 分发][]
* [活动状态 Python 分发][]
* [SciPy-一套科学的 Python 库][]
* [NumPy-Python 的数字库][]
* [Django 项目-成熟的 web 框架/CMS][]
* [IPython 的 Python 高级复制/笔记本][]
* [在 Azure 上 IPython 笔记本][]
* [在 GitHub 上的 Visual Studio 的 Python 工具][]


[生命周期分析 Python 分发]: http://continuum.io
[Enthought Python 分发]: http://www.enthought.com
[活动状态 Python 分发]: http://www.activestate.com
[www.python.org]: http://www.python.org
[www.continuum.io]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy-一套科学的 Python 库]: http://www.scipy.org
[NumPy-Python 的数字库]: http://www.numpy.org
[Django 项目-成熟的 web 框架/CMS]: http://www.djangoproject.com
[IPython 的 Python 高级复制/笔记本]: http://ipython.org
[IPython]: http://ipython.org
[在 Azure 上 IPython 笔记本]: virtual-machines-python-ipython-notebook.md
[云服务]: cloud-services-python-ptvs.md
[网站]: web-sites-python-ptvs-django-mysql.md
[Visual Studio 的 Python 工具]: http://aka.ms/ptvs
[在 GitHub 上的 Visual Studio 的 Python 工具]: https://github.com/microsoft/ptvs
[Python 包索引]: http://pypi.python.org/pypi
[Python 2.7 的 Microsoft Azure SDK]: http://go.microsoft.com/fwlink/?LinkId=254281
[Python 3.4 的 Microsoft Azure SDK]: http://go.microsoft.com/fwlink/?LinkID=516990
[通过 Azure 门户 Linux 虚拟机设置]: create-and-configure-opensuse-vm-in-portal.md
[如何使用 Azure 命令行界面]: crossplat-cmd-tools.md
[创建一个运行 Linux 虚拟机]: virtual-machines-linux-tutorial.md
[Django 使用创建网站]: web-sites-python-create-deploy-django-app.md
[用瓶子创建网站]: web-sites-python-create-deploy-bottle-app.md
[用 Flask 创建网站]: web-sites-python-create-deploy-flask-app.md
[与 Azure 网站配置 Python]: web-sites-python-configure.md
[表存储]: storage-python-how-to-use-table-storage.md
[队列存储]: storage-python-how-to-use-queue-storage.md
[blob 存储]: storage-python-how-to-use-blob-storage.md

测试
