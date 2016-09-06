---
ms.openlocfilehash: 34fec8581e555d71f3adc438b242e243d66345d2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用 Azure 从属服务器插件编写持续集成"
    description="描述如何使用 Azure 从属服务器插件编写持续集成。"
    services="storage" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor="jimbe" />

<tags
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="v-dedomi"/>

#如何使用 Azure 从属服务器插件编写持续集成

编写的 Azure 从属服务器插件使您可以提供从属节点在 Azure 上的运行分布式生成时。

## 安装 Azure 从属服务器插件
1. 在编写面板中，单击**管理编写**。
2. 在**管理编写**页中，单击**管理插件**。
3. 单击**可用**选项卡。
4. 单击**搜索**，键入**Azure**若要将列表限制为相关的插件。

    如果选择的可用插件列表中滚动时，会在**其他**选项卡中**群集管理和分布式生成**部分下发现 Azure 从属服务器插件。
     
5. **Azure 从属服务器**插件选择复选框。
6. 单击**安装**。
7. 重新启动编写。

现在，安装该插件，则接下来的步骤会使用 Azure 订阅配置文件配置插件，并创建从属节点的虚拟机创建模板将使用。


## 您订阅的配置文件配置 Azure 从属服务器插件

订阅配置文件，也称为发布设置，则 XML 文件包含安全凭据，您将需要在您的开发环境中使用 Azure 的某些其他信息。 若要配置 Azure 从属服务器插件，您需要︰ 

* 您的订购 id
* 管理证书为您的订阅的

您订阅的配置文件中，可以找到这些。 如果您没有您预订的配置文件的副本，您可以从[此处](https://manage.windowsazure.com/publishsettings/Index?SchemaVersion=2.0)下载它。 下面是一个示例订阅配置文件。

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

一旦您订阅的配置文件，请按照以下步骤配置 Azure 从属服务器插件。

1. 在编写面板中，单击**管理编写**。
2. 单击**系统配置**。
3. 向下翻页后，可以找到**云**部分。
4. 单击**添加新云 > Microsoft Azure**。



    ![添加新云](./media/azure-slave-plugin-for-hudson/hudson-setup-addcloud.png)

    这将显示的字段，您需要输入您的订阅的详细信息。

    ![配置配置文件](./media/azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png)

5. 复制订阅 id 和管理证书从您的订阅配置文件并将其粘贴在相应字段中。

    当复制的订阅 id 和管理证书，**并不**包括将值括起来的引号。
    
6. 单击**验证配置**。
7. 配置已成功验证，单击**保存**。

## 设置 Azure 从属服务器插件的虚拟机模板

虚拟机模板定义的参数，将使用该插件在 Azure 上创建从属节点。 在以下步骤中我们要创建模板的 Ubuntu VM。 

1. 在编写面板中，单击**管理编写**。
2. 单击**系统配置**。
3. 向下翻页后，可以找到**云**部分。
4. **云**部分中查找**添加 Azure 虚拟机模板**并单击**添加**按钮。



    ![添加虚拟机模板](./media/azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png)

5. 在**名称**字段中指定一个云服务名称。 如果您指定名称所引用的现有的云服务，该服务中将提供虚拟机。 否则，Azure 将创建一个新。 
6. 在**说明**字段中，输入描述要创建的模板的文本。 此信息仅用于跟单的目的，不用于虚拟机的资源调配。
7. 在**标签**字段中，输入**linux**。 此标签用于标识要创建的模板，并随后用于创建编写作业时引用该模板。 
8. 选择将在其中创建虚拟机的区域。
9. 选择相应的虚拟机大小。
10. 指定将在其中创建虚拟机的存储帐户。 请确保它处于同一区域，您将使用在云服务。 如果您想要创建的新存储，您可以将此字段留空。
11. 保留时间指定编写删除空闲从之前的分钟数。 将其保留为默认值为 60。
12. 在**使用状况**中，选择适当的条件时将使用此从属节点。 现在，请选择**此节点尽可能多地利用**。



    此时，您的窗体外观有点类似于这样︰

    ![模板配置](./media/azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png)

13. **图像系列**中的 Id 必须指定您的虚拟机上安装的系统图像。 您可以从列表的图像系列中选择，或指定自定义图像。

    如果您想要选择列表中的图像系列，输入图像系列名称 （区分大小写） 的第一个的字符。 例如，键入**U**将弹出的 Ubuntu 服务器家族的列表。 一旦您从列表中选择，Jenkins 时置备 VM 将使用最新版本的系统映像从该系列。

    ![OS 系列列表](./media/azure-slave-plugin-for-hudson/hudson-oslist.png)
    
    如果您有想要改为使用自定义图像，请输入该自定义映像的名称。 自定义映像的名称未显示在列表中，因此您必须确保正确地输入了名称。    

    对于本教程，键入**U** Ubuntu 图像列表的显示并选择**Ubuntu 服务器 14.04 LTS**。
 
14. **启动方法**，选择**SSH**。
15. 将以下脚本复制并粘贴在**Init 脚本**字段中。

        # Install Java
        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk



        # Install git

        sudo apt-get install -y git



        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    创建虚拟机后，将会执行**Init 脚本**。 在此示例中，脚本安装 Java，git，ant。
    
16. 在**用户名**和**密码**字段中，输入您首选的值将在您的虚拟机创建的管理员帐户。
17. 单击**验证模板**以检查您指定的参数是否有效。
18. 单击**保存**。


## 创建从属节点在 Azure 运行一个编写作业

在本节中，您将创建将在 Azure 上的从属节点运行的编写任务。 

1. 在编写面板中，单击**新作业**。 
2. 输入要创建的作业的名称。
3. 作业类型，选择**生成自由样式软件作业**。
4. 单击**确定**。
5. 在作业配置页中，选择**限制可以运行此项目的位置**。
6. 选择**节点和标签菜单**并选择**linux** （如果在上一节中创建的虚拟机模板，我们指定此标签）。 

7. 在**生成**部分中，单击**添加的生成步骤**，选择**执行 shell**。
8. 编辑下面的脚本**（github 帐户名）**、 **（项目名称）**和**（您的项目目录）**替换为适当的值，并显示在文本区域中粘贴编辑过的脚本。


        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your github account name)/(your project name).git

        fi
        
        # change directory to project

        cd $currentDir/(your project directory)



        #Execute build task

        ant
        
9. 单击**保存**。
10. 在编写仪表板中，找到刚创建和**计划生成**图标上单击该作业。 

编写将创建从设备节点使用上一节中创建的模板，然后执行该任务的生成步骤中指定的脚本。






  

  