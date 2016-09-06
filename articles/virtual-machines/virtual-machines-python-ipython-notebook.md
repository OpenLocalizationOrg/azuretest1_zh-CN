---
ms.openlocfilehash: 89451c5d37c9ae78cb7b2f2c96fa4c2daf869a00
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="IPython 笔记本 |Microsoft Azure"
    description="演示如何部署 Azure，IPython 笔记本使用 Linux 或 Windows 虚拟机 (Vm) 教程。"
    services="virtual-machines"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="python"
    ms.topic="article"
    ms.date="05/20/2015"
    ms.author="huvalo"/>


# 在 Azure 上 IPython 笔记本

[IPython 项目](http://ipython.org)提供了一套科学的计算工具，包括功能强大的交互式解释器、 高性能和易于使用的并行库和调用 IPython 笔记本的基于 web 的环境。 该笔记本提供交互式计算结合实时计算文档创建的代码执行的工作环境。 这些笔记本文件可以包含任意文本、 数学公式、 输入的代码、 结果、 图形、 视频和任何其他类型的介质现代 web 浏览器都能够显示。

无论是绝对新手，Python 和想要了解它在一个有趣的、 交互式的环境或做一些严重的并行/技术计算，IPython 笔记本是个不错的选择。 举例说明了其功能，如下面的屏幕快照所示 IPython 笔记本电脑使用，结合 SciPy 和 Matplotlib 包，分析结构的声音录制。

![Screenshot](./media/virtual-machines-python-ipython-notebook/ipy-notebook-spectral.png)

这篇文章将介绍如何部署 Microsoft Azure，IPython 笔记本使用 Linux 或 Windows 虚拟机 (Vm)。  通过使用 Azure IPython 笔记本，可以轻松地使用 Python 中的所有功能和其许多库提供可伸缩的计算性资源的可通过 web 访问接口。  所有安装在云环境中都完成，因为用户可以访问这些资源，而不需要任何本地超过现代 web 浏览器配置。

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## 创建和配置虚拟机在 Azure 上

第一步是创建一个对 Azure 上运行的虚拟机 (VM)。
此 VM 是完整的操作系统，在云环境中，将用于运行 IPython 笔记本。 Azure 是能够运行 Linux 和 Windows 虚拟机，我们将介绍 IPython 两种类型的虚拟机上安装。

### Linux 虚拟机

按照给定的[此处][门户 vm linux]创建*OpenSUSE*或*Ubuntu*分布的虚拟机。 本教程使用 OpenSUSE 13.2 和 Ubuntu 服务器 14.04 LTS。 我们假设默认用户名称*azureuser*。

### Windows 虚拟机

按照给定的[此处][门户 vm windows]创建*Windows Server 2012 R2 数据中心*分布的虚拟机。 在本教程中，我们假设用户名称是*azureuser*。

## IPython 笔记本中创建终结点

此步骤适用于 Linux 和 Windows 虚拟机。 以后我们将配置 IPython 以其笔记本服务器运行在端口 9999。 若要使此端口可公开获得，我们必须在 Azure 管理门户创建终结点。 此终结点在 Azure 防火墙中的端口会打开，并将映射公共端口 (HTTPS，
443) 为虚拟机 (9999) 的专用端口。

若要创建一个终结点，请转到 VM 的仪表板，请单击**终结点**，然后单击**添加终结点**并创建一个新的终结点 (称为`ipython_nb`在此示例中)。 选择协议、 公用端口**443**和**9999**专用端口的**TCP** 。

![Screenshot](./media/virtual-machines-python-ipython-notebook/ipy-azure-linux-005.png)

此步骤之后，**终结点**仪表板选项卡外观的下一个屏幕快照。

![Screenshot](./media/virtual-machines-python-ipython-notebook/ipy-azure-linux-006.png)

## 在虚拟机上安装必需的软件

在我们的虚拟机上运行 IPython 笔记本，我们必须首先安装 IPython 及其依赖项。

### Linux (OpenSUSE)

若要安装 IPython 及其依赖项，SSH 到 Linux 虚拟机，请完成以下步骤。

使用以下命令安装[NumPy][NumPy]、 [Matplotlib][Matplotlib]、[龙卷风][龙卷风]和其他 IPython 依赖项。

    sudo zypper install python-matplotlib
    sudo zypper install python-tornado
    sudo zypper install python-jinja2
    sudo zypper install ipython

### Linux (Ubuntu)

若要安装 IPython 及其依赖项，SSH 到 Linux 虚拟机，请执行以下步骤。

首先，检索新的包用下面的命令的列表。

    sudo apt-get update

使用以下命令安装[NumPy][NumPy]、 [Matplotlib][Matplotlib]、[龙卷风][龙卷风]和其他 IPython 依赖项。

    sudo apt-get install python-matplotlib
    sudo apt-get install python-tornado
    sudo apt-get install ipython
    sudo apt-get install ipython-notebook

### Windows

要在 Windows 虚拟机上安装 IPython 及其依赖项，请使用远程桌面连接到虚拟机。 然后执行以下步骤，使用 Windows PowerShell 运行所有命令行操作。

**注意**︰ 若要下载使用 Internet Explorer 的任何内容，您将需要更改某些安全设置。  从**服务器管理器**，单击**本地服务器**然后在**IE 增强的安全配置**，管理员将其关闭。  您可以将其重新安装 IPython 之后。

1.  下载并安装最新的 32 位版本的[Python 2.7][]。  您将需要添加`C:\Python27`和`C:\Python27\Scripts`为您`PATH`环境变量。

1.  使用以下命令安装[龙卷风][龙卷风]和[PyZMQ][PyZMQ]和其他 IPython 相关性。

        easy_install tornado
        easy_install pyzmq
        easy_install jinja2
        easy_install six
        easy_install python-dateutil
        easy_install pyparsing

1.  下载并安装使用[NumPy][NumPy] `.exe`在其网站上可用的二进制安装程序。  至本书截稿时止，最新版本的 numpy-1.9.1-win32-superpack-python2.7.exe。

1.  使用下面的命令来安装[Matplotlib][Matplotlib] 。

        pip install matplotlib==1.4.2

1.  下载并安装[OpenSSL][]。

    * 您可以找到所需 Visual C++ 2008年可再发行组件相同的下载页面。

    * 您将需要添加`C:\OpenSSL-Win32\bin`为您`PATH`环境变量。

    > [AZURE.NOTE] 当安装 OpenSSL 使用版本 1.0.1g 或后来因为这些包含了针对 Heartbleed 安全漏洞的修复程序。

1.  安装 IPython 使用下面的命令。

        pip install ipython==2.4

1.  在 Windows 防火墙中打开端口。  在 Windows Server 2012，防火墙将默认情况下阻止传入的连接。  若要打开端口 9999，请执行以下步骤︰

    - 从开始屏幕开始**具有高级安全性的 Windows 防火墙**。

    - 在左面板中单击**入站规则**。

    - 在动作面板中单击**新建规则**。

    - 在新建入站规则向导中选择**端口**。

    - 在下一个屏幕中，选择**TCP** ，在**特定的本地端口**输入**9999** 。

    - 接受默认值，为该规则指定一个名称，然后单击**完成**。

### 配置 IPython 笔记本

接下来，我们将配置 IPython 笔记本。 第一步是创建自定义 IPython 配置文件来封装用以下命令的配置信息。

    ipython profile create nbserver

下一步我们`cd`来创建我们的 SSL 证书和编辑配置文件的配置文件的配置文件目录。

在 Linux 上使用下面的命令。

    cd ~/.ipython/profile_nbserver/

在 Windows 上使用下面的命令。

    cd \users\azureuser\.ipython\profile_nbserver

使用下面的命令来创建 SSL 证书 （Linux 和 Windows）。

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

请注意，因为我们正在创建自行签署式 SSL 证书，当您的浏览器连接到该笔记本将为您提供一条安全警告。  长期生产使用，要使用正确的签名证书与贵组织相关联。  由于证书管理是此演示的范围之内，我们将坚持自行签署式证书现在。

除了使用证书时，您还必须提供密码以防止未经授权使用您的笔记本。  出于安全原因 IPython 使用加密的密码在其配置文件中，因此您将需要首先对密码进行加密。  IPython 提供一个实用程序来做到这一点;在命令提示符下运行以下命令。

    python -c "import IPython;print IPython.lib.passwd()"

这将提示您输入密码并进行确认，并再将打印密码，如下所示。

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

我们将在接下来，编辑该配置文件的配置文件，它是`ipython_notebook_config.py`您是在配置文件目录中的文件。  请注意，此文件可能不存在，就创建它。  此文件的多个字段，默认情况下，所有被注释掉。  您可以使用您喜欢的任何文本编辑器打开此文件，您应该确保它至少具有以下内容。

    c = get_config()

    # This starts plotting support always with matplotlib
    c.IPKernelApp.pylab = 'inline'

    # You must give the path to the certificate file.

    # If using a Linux VM:
    c.NotebookApp.certfile = u'/home/azureuser/.ipython/profile_nbserver/mycert.pem'

    # And if using a Windows VM:
    c.NotebookApp.certfile = r'C:\Users\azureuser\.ipython\profile_nbserver\mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port

    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### 运行 IPython 笔记本

此时，我们就可以开始 IPython 笔记本了。 要做到这一点，导航到要存储笔记本中的，并使用下面的命令启动 IPython 笔记本服务器的目录。

    ipython notebook --profile=nbserver

您现在应该能够访问地址处 IPython 笔记本`https://[Your Chosen Name Here].cloudapp.net`。

第一次访问您的笔记本，登录页会询问您的密码。

![Screenshot](./media/virtual-machines-python-ipython-notebook/ipy-notebook-001.png)

并且，一旦登录，您将看到 IPython 笔记本仪表板，这是所有笔记本操作的控制中心。  可以在此页创建新的笔记本和打开现有的。

![Screenshot](./media/virtual-machines-python-ipython-notebook/ipy-notebook-002.png)

如果您单击**新建笔记本**按钮，您将看到下面的起始页。

![Screenshot](./media/virtual-machines-python-ipython-notebook/ipy-notebook-003.png)

区域标有`In []:`提示是输入的区域中，您可以在此处键入任何有效的 Python 代码和击中时将执行`Shift-Enter`或单击"播放"图标 （右指三角工具栏中）。

因为我们已经配置了笔记本电脑 NumPy 和 Matplotlib 开始自动支持，甚至可以生成图表中的下一个屏幕快照所示。

![Screenshot](./media/virtual-machines-python-ipython-notebook/ipy-notebook-004.png)

## 功能强大的范例︰ 实时计算文档与富媒体

笔记本本身应该感觉非常自然，给人已使用 Python 和字处理器，因为它是在某些方面两者兼︰ 您可以执行的 Python 代码块，但也可以将保留注释和其他文本从"代码"到"减价"的单元格的样式使用工具栏中的下拉列表菜单。

![Screenshot](./media/virtual-machines-python-ipython-notebook/ipy-notebook-005.png)


但这并不仅仅是文字处理，IPython 笔记本允许混合计算和富媒体 （文本、 图形、 视频和几乎任何现代 web 浏览器可以显示）。 例如，可以为教学目的与计算混合说明性视频。

![Screenshot](./media/virtual-machines-python-ipython-notebook/ipy-notebook-006.png)

或者，在下一个屏幕快照中所示，嵌入保持实时和便于使用笔记本文件内的外部网站。

![Screenshot](./media/virtual-machines-python-ipython-notebook/ipy-notebook-007.png)

和有关科学和技术的 Python 的许多优秀库的功能与计算，在下面的屏幕快照，可以使用相同的易于为复杂网络分析，在一个环境中的所有执行简单的计算。

![Screenshot](./media/virtual-machines-python-ipython-notebook/ipy-notebook-008.png)

此范例中的混合实时计算与现代网络的威力提供了许多可能性，并适合于云;可以使用笔记本︰

* 作为计算的便笺本记录探索处理问题。

* 与在实时计算窗体或硬拷贝格式 （HTML、 PDF） 中的同事共享的结果。

* 若要分发并提供实时的教学资料，涉及计算，以便学生可以立即进行试验的实际代码，对其进行修改并重新以交互方式执行。

* 提供"可执行白皮书"，以一种可以立即复制、 验证和扩展由他人提供的研究结果。

* 作为一个平台，用于协作计算︰ 多个用户可以登录到同一台笔记本服务器共享实时计算会话。



如果您转到 IPython 来源代码[存储库中][]，您会发现使用笔记本的示例，您可以下载并然后试验 Azure IPython VM 上整个目录。  只需下载`.ipynb`网站中的文件和将它们上载到笔记本 Azure VM 的仪表板 （或直接到 VM 下载它们）。

## 结论

IPython 笔记本提供功能强大的界面以交互方式访问在 Azure 上的 Python 生态系统的能力。  它涵盖范围广泛的使用情况，包括简单的探索和学习 Python、 数据分析和可视化效果、 模拟和并行计算。 结果笔记本文档包含计算的执行并与其他 IPython 用户可以共享的完整记录。  IPython 笔记本可以用作本地应用程序，但是它非常适合在 Azure 的云部署

在[Visual Studio 的 Python 工具][](PTVS) 通过 Visual Studio 内还有 IPython 的核心功能。 PTVS 是一个免费和开源插件的 Microsoft，变成 Visual Studio 包括 IntelliSense，调试，使用高级的编辑器高级 Python 开发环境分析和并行计算的集成。




[龙卷风]:      http://www.tornadoweb.org/          "龙卷风"
[PyZMQ]:        https://github.com/zeromq/pyzmq     "PyZMQ"
[NumPy]:        http://www.numpy.org/               "NumPy"
[Matplotlib]:   http://matplotlib.sourceforge.net/  "Matplotlib"
[门户网站的虚拟机窗口]: /manage/windows/tutorials/virtual-machine-from-gallery/
[门户网站的虚拟机-linux]: /manage/linux/tutorials/virtual-machine-from-gallery/
[存储库]: https://github.com/ipython/ipython
[visual studio 的 python 工具]: http://aka.ms/ptvs
[Python 2.7]: http://www.python.org/download
[OpenSSL]: http://slproweb.com/products/Win32OpenSSL.html
