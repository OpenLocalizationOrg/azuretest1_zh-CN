---
ms.openlocfilehash: 45a022829ec7e1f1159d0fe5f3ad3aca0340218b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
#如何使用 Mac 和 Linux 的 Azure 命令行工具

本指南介绍了如何使用 Mac 和 Linux 的 Azure 命令行工具来创建和管理 Azure 中的服务。 所包含的方案包括**安装工具**、**导入您的发布设置**、**创建和管理 Azure 网站**和**创建和管理 Azure 的虚拟机**。 完整的参考文档，请参阅[Azure 的命令行工具，用于 Mac 和 Linux 文档][参考文档]。 

##目录
* [Mac 和 Linux 的 Azure 命令行工具有哪些？](#Overview)
* [如何为 Mac 和 Linux 安装 Azure 命令行工具](#Download)
* [如何创建 Azure 帐户](#CreateAccount)
* [如何下载并导入发布设置](#Account)
* [如何创建和管理一个 Azure 的 Web 站点](#WebSites)
* [如何创建和管理 Azure 的虚拟机](#VMs)


<h2><a id="Overview"></a>Mac 和 Linux 的 Azure 命令行工具有哪些？</h2>

Mac 和 Linux 的 Azure 命令行工具是一套用于部署和管理 Azure 服务命令行工具。
 
支持的任务如下︰

* 导入发布设置。
* 创建和管理 Azure 网站。
* 创建和管理 Azure 的虚拟机。

支持的命令的完整列表，请键入`azure -help`在命令行安装工具之后, 或[参考文档][参考文档]，请参阅。

<h2><a id="Download">如何为 Mac 和 Linux 安装 Azure 命令行工具</a></h2>

下面的列表包含安装的命令行工具，具体取决于您的操作系统的信息︰

* **Mac**︰ 下载[Azure SDK 安装程序][mac 安装程序]。 打开下载的.pkg 文件，请按照提示完成安装步骤。

* **Linux**︰ 安装[Node.js][nodejs 组织]的最新版本 （请参见[安装 Node.js 通过程序包管理器][安装节点 linux]），然后运行以下命令︰

        npm install azure-cli -g

    **注意**︰ 您可能需要使用提升的权限运行此命令︰

        sudo npm install azure-cli -g

* **Windows**︰ 运行 Winows 安装程序 （.msi 文件），该主题可在此处︰ [Azure 命令行工具][windows 安装程序]。


若要测试的安装，请键入`azure`在命令提示符下。 如果安装成功，您将看到所有可用项的列表`azure`命令。

<h2><a id="CreateAccount"></a>如何创建 Azure 帐户</h2>

Mac 和 Linux 使用 Azure 命令行工具，您将需要 Azure 帐户。

打开 web 浏览器和浏览到[http://www.windowsazure.com][windowsazuredotcom]然后单击右上角的**免费试用版**。

![Azure 网站][Azure Web Site]

按照说明创建帐户。

<h2><a id="Account"></a>如何下载并导入发布设置</h2>

若要开始，您需要首先下载并导入您的发布设置。 这样，如果您要使用的工具来创建和管理 Azure 服务。 下载您的发布设置，请使用`account download`命令︰

    azure account download

这将打开默认浏览器，并提示您登录到管理门户。 中，签名后您`.publishsettings`文件将被下载。 请记下该文件的保存位置。

接下来，导入`.publishsettings`通过运行下面的命令文件替换`{path to .publishsettings file}`的路径与您`.publishsettings`文件︰

    azure account import {path to .publishsettings file}

您可以删除所有存储的信息<code>import</code>命令通过使用<code>account clear</code>命令︰

    azure account clear

若要查看选项列表`account`命令，使用`-help`选项︰

    azure account -help

导入后您发布设置，则应删除`.publishsettings`文件出于安全考虑。

> [AZURE.NOTE] 当您导入的发布设置、 访问 Azure 订购的凭据存储在您`user`文件夹。 您`user`文件夹保护您的操作系统。 但是，建议您采取其他步骤来加密您`user`文件夹。 您可以通过以下方式执行操作︰    
> 
> - 在 Windows 中，可以修改文件夹属性或使用 BitLocker。
> - 在 Mac，打开 FileVault 文件夹。
> - 在 Ubuntu 中，使用加密主目录功能。 其他 Linux 发行版本提供等效的功能。

现在您可以为正在创建和管理 Azure 网站和 Azure 的虚拟机。  

<h2><a id="WebSites"></a>如何创建和管理 Azure 网站</h2>

###创建网站

若要创建一个 Azure 网站，首先创建一个空的目录称为`MySite`，然后浏览到该目录。

然后，运行下面的命令︰

    azure site create MySite --git

此命令的输出将包含新创建的网站的默认 URL。 `--git`选项允许您使用 git 发布到您的网站通过创建两个本地应用程序目录中，并在您的网站的数据中心的 git 存储库。 请注意，是否您的本地文件夹已经是一个 git 存储库中，该命令将到现有存储库中，指向您的网站数据中心存储库中添加新的远程。

请注意，您可以执行`azure site create`命令与以下任何选项︰

* `--location [location name]`. 此选项允许您指定您的网站创建 （例如"西美国"） 所在的数据中心的位置。 如果省略此选项，将出现提示选择一个位置。
* `--hostname [custom host name]`. 此选项允许您指定为您的网站自定义主机名。

然后可以向网站目录中添加内容。 使用常规的 git 流 (`git add`， `git commit`) 以提交您的内容。 使用下面的 git 命令以将您的网站的内容推送到 Azure: 

    git push azure master

###从 GitHub 设置发布

若要设置连续发布从 GitHub 存储库，使用`--GitHub`选项创建网站时︰

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

如果有本地 GitHub 存储库的克隆或已使用 GitHub 存储库中的单个远程引用库，此命令将自动发布 GitHub 存储库对您的站点中的代码。 从此以后，推送到 GitHub 存储库中的任何更改将自动发布到您的网站。

从 GitHub 发布时，使用默认分支是主分支。 若要指定不同的分支，请从您的本地存储库中执行以下命令︰

    azure site repository <branch name>

###配置应用程序设置

应用程序设置是可用于在运行时应用程序的键 / 值对。 当为 Azure 网站设置，应用程序设置值将用相同的密钥，您的站点的 Web.config 文件中定义覆盖设置。 对于 Node.js 和 PHP 应用程序，应用程序设置可作为环境变量。 下面的示例演示如何设置键 / 值对︰

    azure site config add <key>=<value> 

若要查看列表中的所有键/值对，请使用以下︰

    azure site config list 

或者，如果您知道密钥，并且想要查看值，可以使用︰

    azure site config get <key> 

如果您想要更改现有项的值必须先清除现有的密钥，然后重新添加它。 清除的命令是︰

    azure site config clear <key> 

###列出并显示站点

若要列出您的网站，请使用下面的命令︰

    azure site list

若要获取有关某个站点的详细的信息，请使用`site show`命令。 下面的示例显示详细信息`MySite`:

    azure site show MySite

###停止、 启动或重新启动站点

您可以停止、 启动或重新启动的网站`site stop`， `site start`，或`site restart`命令︰

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

###删除网站

最后，您可以删除的网站`site delete`命令︰

    azure site delete MySite

请注意，如果您正在运行任何命令从上面运行的文件夹内`site create`，您不需要指定的站点名称`MySite`作为最后的参数。

若要查看完整的列表`site`命令，使用`-help`选项︰

    azure site -help 

<h2><a id="VMs"></a>如何创建和管理 Azure 的虚拟机</h2>

Azure 的虚拟机创建虚拟机映像 （.vhd 文件） 提供或库中的图像可用。 若要查看可用的图像，请使用`vm image list`命令︰

    azure vm image list

可以设置并启动虚拟机从一个可用图像与`vm create`命令。 下面的示例演示如何创建一个 Linux 虚拟机 (称为`myVM`) 从图像中图像库 (CentOS 6.2)。 根用户名称和密码为虚拟机`myusername`，`Mypassw0rd`分别。 (请注意，`--location`参数指定在其中创建虚拟机的数据中心。 如果省略`--location`参数，则将提示您选择的位置。)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

您可能会考虑通过`--ssh`标志 (Linux) 或`--rdp`标志 (Windows)`vm create`要启用远程连接到新创建的虚拟机。

而是将提供的自定义图像中的虚拟机，如果可以从使用.vhd 文件创建的图像`vm image create`命令，然后使用`vm create`命令来配置虚拟机。 下面的示例演示如何创建一个 Linux 映像 (称为`myImage`) 从本地.vhd 文件。 (`--location`参数指定将图像存储在其中的数据。)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

而不是创建从本地.vhd 映像，您可以从存储在 Azure Blob 存储.vhd 创建图像。 你可以用`blob-url`参数︰

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

创建图像后, 即可调配虚拟机映像中使用的`vm create`。 下面的命令创建称为虚拟机`myVM`从上面创建的图像 (`myImage`)。

    azure vm create myVM myImage myusername --location "West US"

配置虚拟机之后，可能要创建终结点以允许远程访问您的虚拟机 （例如）。 下面的示例使用`vm create endpoint`命令以打开外部端口 22 和本地端口 22 上`myVM`:

    azure vm endpoint create myVM 22 22

您可以详细的信息 （包括 IP 地址、 DNS 名称和终结点信息） 虚拟机与`vm show`命令︰

    azure vm show myVM

关机，启动，或重新启动虚拟机，请使用下列命令之一︰

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

最后，删除虚拟机，请使用`vm delete`命令︰

    azure vm delete myVM

有关创建和管理虚拟机的命令的完整列表，请使用`-h`选项︰

    azure vm -h

<!-- IMAGES -->
[Azure 网站]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs 组织]: http://nodejs.org/
[安装节点 linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac 安装程序]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows 安装程序]: http://go.microsoft.com/fwlink/?LinkID=275464
[参考文档]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com

