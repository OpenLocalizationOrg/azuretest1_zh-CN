---
ms.openlocfilehash: b7d247d16a86eb939cea110a0a80004a948df1f4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用 Azure 的从属服务器插件 Jenkins 持续集成 |Microsoft Azure"
    description="描述如何使用 Azure 的从属服务器插件 Jenkins 持续集成。"
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

# 如何使用 Azure 的从属服务器插件 Jenkins 持续集成

可用于 Azure 的从属服务器插件 Jenkins 于规定从属节点在 Azure 上运行分布式生成时。

## 若要安装该插件的 Azure 从属服务器
1. 在 Jenkins 面板中，单击**管理 Jenkins**。
2. 在**管理 Jenkins**页上，单击**管理插件**。
3. 单击**可用**选项卡。
4. 在上面的可用插件列表筛选器字段中，键入**Azure**来限制对相关插件的列表。

    如果您选择的可用插件列表中滚动，您会发现 Azure 从属服务器插件**群集管理和分布式生成**部分的下面。

5. 选择**Azure 从属服务器插件**复选框。
6. 单击**安装时不重新启动**或**立即下载并安装后重新启动**。

既然该插件已安装下, 一步是配置插件使用 Azure 订阅配置文件并创建从设备节点的虚拟机创建模板将使用。


## 与订阅配置文件配置插件 Azure 辅

订阅配置文件，也称为发布设置，则 XML 文件包含安全凭据，您将需要在您的开发环境中使用 Azure 的某些其他信息。 若要配置插件 Azure 的从属服务器，您需要︰

* 您的订购 id
* 管理证书为您的订阅的

您订阅的配置文件中，可以找到这些。 如果您没有您预订的配置文件的副本，您可以下载它从[订阅站点](https://manage.windowsazure.com/publishsettings/Index?SchemaVersion=2.0)。 下面是一个示例订阅配置文件。

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID value>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

您预订的个人资料后，按照以下步骤来配置 Azure 的从属服务器插件︰

1. 在 Jenkins 面板中，单击**管理 Jenkins**。
2. 单击**系统配置**。
3. 向下翻页后，可以找到**云**部分。
4. 单击**添加新云 > Microsoft Azure**。

    ![云的部分](./media/azure-slave-plugin-for-jenkins/jenkins-cloud-section.png)

    这将显示的字段，您需要输入您的订阅的详细信息。

    ![配置订阅](./media/azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png)

5. 从您的订阅配置文件复制订阅 id 和管理证书的值并将其粘贴在相应字段中。

    当复制的订阅 id 和管理证书，不包括将值括起来的引号。

6. 单击**验证配置**。
7. 配置验证正确，单击**保存**。

## 若要设置虚拟机模板的 Azure 从属服务器插件

虚拟机模板定义插件将使用 Azure 创建从属节点的参数。 在以下步骤中，我们将创建为 Ubuntu 虚拟机模板。

1. 在 Jenkins 面板中，单击**管理 Jenkins**。
2. 单击**系统配置**。
3. 向下翻页后，可以找到**云**部分。

4. 在**云**部分找到**添加 Azure 虚拟机模板**，，然后单击**添加**。

    ![添加虚拟机模板](./media/azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png)

    这将显示您在其中输入有关要创建的模板的详细信息的字段。

    ![空的常规配置](./media/azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png)

5. 在**名称**框中，输入 Azure 的云服务名称。 如果您输入的名称是指现有的云服务，该服务中将提供虚拟机。 否则，Azure 将创建一个新。

6. 在**说明**框中，输入描述要创建的模板的文本。 这仅适用于您的记录，并不用于虚拟机的资源调配。
7. **标签**框中用来标识要创建的模板，并随后用于创建 Jenkins 作业时引用该模板。 我们的目的，在此框中输入**linux** 。
8. 在**区域**列表中，单击将在其中创建虚拟机的地区。
9. 在**虚拟机大小**列表中，单击适当的大小。
10. 在**存储帐户名**框中，指定将在其中创建虚拟机的存储帐户。 请确保它处于同一区域，您将使用在云服务。 如果您想要创建的新存储，可以将此框保留为空。
11. 保留时间指定 Jenkins 删除空闲从之前的分钟数。 将其保留为默认值为 60。 您还可以选择关闭该从属服务器，而不是删除它在空闲时。 要做到这一点，选择**关闭只 （不删除） 后保留时间**复选框。
12. 在**使用**列表中，单击适当的条件时将使用此从属节点。 现在，请单击**此节点尽可能多地利用**。

    此时，窗体应看起来有点类似于这样︰

    ![检查点一般的模板配置](./media/azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png)

    下一步是提供有关要您辅以在中创建的操作系统映像的详细信息。

13. 在**图像系列或 Id**框中，您需要指定哪些系统映像的安装在虚拟机上。 您可以从列表的图像系列中选择，或指定自定义图像。

    如果您想要选择列表中的图像系列，输入图像系列名称 （区分大小写） 的第一个的字符。 例如，键入**U**将弹出的 Ubuntu 服务器家族的列表。 从列表中选择后，Jenkins 时提供您的虚拟机将使用最新版本的系统映像从该系列。

    ![OS 映像的列表示例](./media/azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png)

    如果您有想要改为使用自定义图像，请输入该自定义映像的名称。 自定义映像名称不显示在列表中，因此您必须确保正确地输入了名称。

    对于本教程，键入**U** ，弹出的 Ubuntu 图像列表，然后单击**Ubuntu 14.04 LTS 服务器**。

14. 在**启动方法**列表中，单击**SSH**。
15. 将以下脚本复制并粘贴在**Init 脚本**中。

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

    创建虚拟机后，将会执行 init 脚本。 在此示例中，脚本安装 Java，Git，ant。

16. 在**用户名**和**密码**框中，输入您首选的值将在虚拟机创建的管理员帐户。
17. 单击要检查您指定的参数是否有效的**验证模板**。
18. 单击**保存**。


## 若要创建从属节点在 Azure 运行 Jenkins 作业

在本节中，您将创建 Jenkins 任务将在 Azure 上的从属节点上运行。 您将需要包含您自己的项目最多，在 GitHub 要继续下去。

1. Jenkins 仪表板，请单击**新项目**。
2. 输入要创建任务的名称。
3. 项目类型，请单击**自由式项目**。
4. 单击**确定**。
5. 在任务配置页上，选中**限制可以运行此项目的位置**。
6. 在**标签表达式**框中，输入**linux**。 在前面的部分中，我们创建了，我们命名**linux**，这是我们的指定的从属服务器模板。
7. 在**生成**部分中，单击**添加的生成步骤**，选择**执行 shell**。
8. 编辑下面的脚本**（GitHub 帐户名）**、 **（项目名称）**和**（您的项目目录）**替换为适当的值，并显示在文本区域中粘贴编辑过的脚本。

        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your GitHub account name)/(your project name).git

        fi

        # change directory to project

        cd $currentDir/(your project directory)



        #Execute build task

        ant

9. 单击**保存**。
10. Jenkins 仪表板，在将鼠标悬停在您刚刚创建的任务，请单击下拉箭头以显示任务选项。
11. 单击**生成现在**。

Jenkins 将使用上一节中创建的模板创建从属节点，然后执行该任务的生成步骤中指定的脚本。
