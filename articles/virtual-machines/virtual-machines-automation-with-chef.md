---
ms.openlocfilehash: 31c7aaa5e5ad58ffcd2c1ce810accf6f5152010d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="自动执行与厨师的简历 Azure 的虚拟机部署"
   description="了解使用厨师的简历的 Azure 的虚拟机实现自动化的画"
   services="virtual-machines"
   documentationCenter=""
   authors="diegoviso"
   manager="timlt"
   editor=""/>

<tags ms.service="virtual-machines" ms.workload="infrastructure-services"
ms.tgt_pltfrm="vm-multiple"
ms.devlang="na"
ms.topic="article"
ms.date="05/19/2015"
ms.author="diviso"/>

# 自动执行与厨师的简历 Azure 的虚拟机部署

厨师的简历是提供自动化的理想工具，和所需状态的配置。

我们最新的云 api 版本中，厨师的简历提供与 Azure，无缝集成使您能够设置和部署配置状态，通过一个命令。

在本文中，我将展示如何设置您的厨师的简历环境设置 Azure 的虚拟机并引导您完成创建策略或"菜谱"，再到 Azure 的虚拟机中部署此菜谱。

让我们开始 ！

## 厨师的简历基础知识

在开始之前，建议您检查厨师的简历的基本概念。 没有极好的材料<a href="http://www.chef.io/chef" target="_blank">这里</a>和我建议尝试本演练之前，您有快速阅读。 在我们开始之前，我将但是回顾基础知识。

下图显示了高级厨师的简历体系结构。

![][2]

厨师的简历有三个主要的体系结构组件︰ 厨师的简历服务器、 客户端厨师的简历 （节点） 和厨师的简历工作站。

厨师的简历服务器是我们管理的观点，有厨师的简历服务器的两个选项︰ 托管的解决方案或非内部解决方案。 我们将使用托管的解决方案。

厨师的简历客户端 （节点） 是位于您正在管理的服务器的代理。

厨师的简历工作站是我们管理工作站在我们创建我们的策略和执行管理命令。 我们从厨师的简历工作站来管理我们的基础结构运行**刀子**的命令。

此外，还有"烹调书"和"处方"的概念。 实际上，这些是我们定义并应用到我们的服务器的策略。

## 正在准备工作站

首先，我们准备工作站。 我正在使用标准的 Windows 工作站。 我们需要创建一个目录来存储我们的配置文件和烹调书。

首先创建名为 C:\chef 的目录。

然后，创建一个名为 c:\chef\cookbooks 的第二个目录。

我们现在需要这样厨师的简历会与我们 Azure 订阅下载我们 Azure 的设置文件。

下载您发布从[此处](https://manage.windowsazure.com/publishsettings/)的设置。

在 C:\chef 中保存发布设置文件。

##创建帐户由他人管理厨师的简历

注册托管的厨师的简历帐户[此处](https://manage.chef.io/signup)。

在注册过程中，您将需要创建新的组织。

![][3]

您的组织创建后，下载的初学者工具包。

![][4]

> [AZURE.NOTE] 如果您收到一条提示，警告您您的密钥将被重置，则确定后尚未配置任何现有的基础结构，我们继续。

此初学者工具包 zip 文件包含您的组织配置文件和键。

##厨师的简历工作站配置

厨师的简历-starter.zip 到 C:\chef 的内容提取。

复制所有文件在下，厨师的简历-starter\chef-repo\.厨师 c:\chef 目录的简历。

您的目录应该现在类似于下面的示例。

![][5]

您现在应该拥有 Azure 的发布文件包括在根目录下的 c:\chef 的四个文件。

PEM 文件包含您的组织和管理私钥进行通信而 knife.rb 文件中包含您的刀子配置。 我们将需要编辑 knife.rb 文件。

您选择的编辑器中打开该文件，并通过删除修改"cookbook_path"/.从使它出现如下所示的路径。

    cookbook_path  ["#{current_dir}/cookbooks"]

此外将添加以下行，以反映您 Azure 的名称发布设置文件。

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

您的 knife.rb 文件现在应类似于下面的示例。

![][6]

刀子引用下 c:\chef\cookbooks 的烹调书目录，并且还在 Azure 的操作过程中使用我们的 Azure 发布设置文件将确保这些行。

## 安装厨师的简历开发工具包

下一步，[下载并安装](http://downloads.getchef.com/chef-dk/windows)ChefDK （厨师的简历开发工具包），可以设置您的厨师的简历工作站。

![][7]

安装在默认位置的 c:\opscode。 此安装需要大约 10 分钟。

确认您的 PATH 变量包含的项目 C:\opscode\chefdk\bin;C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

如果它们不存在，请确保您添加这些路径 ！

*请注意，路径的顺序很重要 ！* 如果 opscode 路径不按正确的顺序会有问题。

在继续操作之前，请重新启动您的工作站。

接下来，我们将安装刀子 Azure 扩展。 这提供了与"Azure 插件"的刀子。

运行以下命令。

    chef gem install knife-azure ––pre

> [AZURE.NOTE] – 前参数可确保您收到的刀子 Azure 插件提供的最新一套 Api 访问最新 RC 版本。

很可能在同一时间同时安装依赖项的数量。

![][8]


要确保一切都正确配置，请运行下面的命令。

    knife azure image list

如果一切配置正确，您将看到可用的 Azure 图像滚动列表。

祝贺您。 工作站设置了 ！

##创建手册

菜谱是厨师的简历用于定义一组要在托管客户端上执行的命令。 创建一本非常简单，我们使用**厨师的简历生成菜谱**中的命令来生成我们的菜谱模板。 我想自动部署 IIS 的策略，我将会呼叫我菜谱的 web 服务器。

在 C:\Chef 目录下运行下面的命令。

    chef generate cookbook webserver

这将生成一组 C:\Chef\cookbooks\webserver 目录下的文件。 我们现在需要定义的一套我们希望我们的厨师的简历客户我们托管的虚拟机上执行的命令。

此命令存储在文件 default.rb。 在此文件中，我将定义一组命令，安装 IIS，启动 IIS 并将模板文件复制到 wwwroot 文件夹。

修改 C:\chef\cookbooks\webserver\recipes\default.rb 文件，并添加以下行。

    powershell_script 'Install IIS' do
        action :run
        code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
        action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
        source 'Default.htm.erb'
        rights :read, 'Everyone'
    end

完成后，请保存该文件。

## 创建模板

我们前面已提到，我们需要生成一个模板文件，它将作为我们 default.html 页。

运行以下命令以生成模板。

    chef generate template webserver Default.htm

现在，请导航到 C:\chef\cookbooks\webserver\templates\default\Default.htm.erb 文件。 通过添加一些简单的"Hello World"HTML 代码中编辑该文件，然后保存该文件。



## 厨师的简历服务器上传菜谱

在此步骤中，我们已采取的菜谱，我们已在我们的本地计算机上创建副本并将其上载到厨师的简历托管服务器。 一旦上传，手册将出现在**策略**选项卡中。

    knife cookbook upload webserver

![][9]

## 部署虚拟机与美工刀 Azure

现在，我们将部署 Azure 的虚拟机，并应用"web 服务器"手册，这将在安装 IIS web 服务和默认网页。

为此，使用**刀子 azure 服务器创建**命令。

在该命令的示例将显示下一步。

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

参数是显而易见的。 替换为您特定的变量，并运行。

> [AZURE.NOTE] 通过命令行中，我还自动化网络终结点筛选器规则使用 – tcp 端点参数。 我已经打开端口 80 和 3389 可以访问到我的 web 页和 RDP 会话。

一旦运行该命令，请转到 Azure 的门户，您将看到计算机开始提供。

![][13]

在命令提示符下显示下一步。

![][10]

部署完成后，我们应该能够通过 80 端口连接到 web 服务，因为我们曾当我们设置虚拟机使用刀子 Azure 命令打开该端口。 由于此虚拟机是云服务中唯一的虚拟机，我会使用云服务 url 连接它。

![][11]

正如您所看到的有创造性与我的 HTML 代码。

别忘了我们还可以通过从 Azure 门户通过端口 3389 RDP 会话连接。

我希望这是有帮助的 ！ 转并启动您的基础结构代码使用 Azure 的旅程今天 ！


<!--Image references-->
[2]: ./media/virtual-machines-automation-with-chef/2.png
[3]: ./media/virtual-machines-automation-with-chef/3.png
[4]: ./media/virtual-machines-automation-with-chef/4.png
[5]: ./media/virtual-machines-automation-with-chef/5.png
[6]: ./media/virtual-machines-automation-with-chef/6.png
[7]: ./media/virtual-machines-automation-with-chef/7.png
[8]: ./media/virtual-machines-automation-with-chef/8.png
[9]: ./media/virtual-machines-automation-with-chef/9.png
[10]: ./media/virtual-machines-automation-with-chef/10.png
[11]: ./media/virtual-machines-automation-with-chef/11.png
[13]: ./media/virtual-machines-automation-with-chef/13.png


<!--Link references-->
