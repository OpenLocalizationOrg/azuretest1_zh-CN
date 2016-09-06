---
ms.openlocfilehash: 1f572876e10bcf116648ed32ab7dfaaa80c4ba53
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在 Azure 远程应用程序使用的任何设备上运行任何 Windows 应用程序"
   description="了解如何与您的用户通过使用 Azure 远程应用程序共享任何 Windows 应用程序。"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="09/02/2015"
   ms.author="elizapo"/>

# 在 Azure 远程应用程序使用的任何设备上运行任何 Windows 应用程序

您可以运行 Windows 应用程序任意位置在任何设备上，现在，严重的只是通过使用 Azure RemoteApp。 无论是 Internet Explorer 6，10 年前或 Office 应用程序中的用户不再需要依赖于特定的操作系统 （如 Windows XP) 的几个应用，则编写一个自定义的应用程序。

Azure 远程应用程序，您的用户还可以使用他们自己的 Android 或苹果设备并获得相同的体验，在 Windows 上 （或在 Windows 手机上），他们拥有的资源。 这可以通过 Windows 应用程序的 Windows Azure 上的虚拟机的集合中所在-您的用户可以访问他们具有互联网连接从任何位置。 

阅读有关如何执行此操作的示例。

在本文中，我们将与所有用户都共享的访问权限。 但是，您可以使用任何应用程序。 只要您可以在 Windows Server 2012 R2 的计算机上安装您的应用程序，可以使用下面的步骤将其共享。 您可以查看[应用程序的要求](remoteapp-appreqs)，以确保您的应用程序将工作。

请注意，因为访问是一个数据库，并且我们希望该数据库很有用，我们将几个额外的步骤以允许用户访问的权限访问数据共享。 如果您的应用程序不是一个数据库，或不需要您的用户能够访问的文件共享，您可以跳过那些步骤在本教程中

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]


## 在远程应用程序中创建收藏集

首先创建一个集合。 集合是一个用于您的应用程序和用户。 基于图像的每个集合-可以创建您自己也可以使用一个提供与您的订阅。 在本教程中，我们使用的 Office 2013 试用图像-它包含了我们想要共享的应用程序。

1. 在 Azure 的门户中，向下滚动左侧导航树，直到您看到远程应用程序。 打开该网页。
2. 单击**创建一个远程应用程序集合**。
3. 单击**快速创建**并输入您的收藏集的名称。
4. 选择要用于创建您的集合的区域。 为获得最佳体验，选择在地理位置上与您的用户访问应用程序的位置的位置最接近的地区。 例如，在本教程中，用户将位于华盛顿州雷蒙德市。 最近的 Azure 区域是**西部美国**。
5. 选择您要使用的付费计划。 基本的付费计划放 16 个用户大 Azure VM，虽然标准提供商大 Azure VM 上有 10 个用户。 作为一个常规的示例中，基本的规划工作特别适合于数据条目类型工作流。 希望生产力应用程序中，如办公场所、 标准的计划。
6. 最后，选择 Office 2013 专业图像。 此映像包含 Office 2013 应用程序。 只是提醒-此图像才试用集合和 Poc 很好。 您 ' 生产集合中不能使用此映像。
7. 现在，单击**创建远程应用程序集合**。

![在远程应用程序中创建一个云集合](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

此时开始创建您的收藏，但它可能要花一个小时的时间。

现在您就可以添加您的用户。

## 与用户共享应用程序

一旦您已经创建成功，就可以发布到用户的访问权限，并添加应具有访问权限的用户。

如果在创建集合时，已经离开 Azure 远程应用程序节点，，首先使您追根溯源，它从 Azure 的主页。

2. 单击您以前访问其他选项，配置集合创建的集合。
![新的 RemoteApp 云集合](./media/remoteapp-anyapp/ra-anyappcollection.png)
3. 在**发布**选项卡上，单击**发布**屏幕的底部，然后单击**发布开始菜单程序**。
![发布的 RemoteApp 程序](./media/remoteapp-anyapp/ra-anyapppublish.png)
4. 选择要从列表发布的应用程序。 对于我们的目的，我们选择了访问。 单击**完成**。 等待应用程序完成发布。
![在远程应用程序发布的访问](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)


1. 一旦应用程序已完成出版，头通过对**用户访问权**选项卡添加需要访问您的应用程序的所有用户。 输入您的用户的用户名 （电子邮件地址），然后单击**保存**。

![将用户添加到远程应用程序](./media/remoteapp-anyapp/ra-anyappaddusers.png)


1. 现在，就应该告诉这些新的应用程序以及如何访问它们的用户。 若要执行此操作，请发送您的用户它们指向远程桌面客户端下载 URL 的电子邮件。
![客户端的远程应用程序下载的 URL](./media/remoteapp-anyapp/ra-anyappurl.png)

## 配置访问权

通过远程应用程序部署之后，一些应用程序需要额外的配置。 特别是，进行访问，我们将对任何用户都可以访问的 Azure 上创建的文件共享。 (如果您不想要这样做，您可以创建一个[混合集合](remoteapp-create-hybrid-deployment.md)[而不是我们云集合]，以便当用户访问的文件和您的本地网络上的信息。)然后，我们需要告诉我们的用户在其计算机上的本地驱动器映射到 Azure 的文件系统。

您以管理员身份第一部分。 然后，我们有一些步骤为您的用户。

1. 通过发布命令行界面 (cmd.exe) 开始。 在**发布**选项卡中选择**命令**，然后单击**发布 > 使用路径的发布程序**。
2. 输入的应用程序和路径的名称。 我们的目的，使用路径"文件资源管理器"作为名称并选择"%systemdrive%\windows\explorer.exe"。
![Cmd.exe 文件发布。](./media/remoteapp-anyapp/ra-publishcmd.png)
3. 现在，您需要创建一个 Azure[存储帐户](../storage-create-storage-account.md)。 名为"accessstorage，"我们因此请选择对您有意义的名称。 （以 misquote Highlander，可以有一个"accessstorage。"）![我们 Azure 存储帐户](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. 现在返回到您的仪表板使您可以获取的路径存储 （终结点位置）。 您将使用此中一位，所以请确保地方复制。
![存储帐户路径](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. 接下来，一旦创建存储帐户，您将需要主访问键。 单击**管理访问密钥**，然后复制主访问键。
6. 现在，设置存储帐户的上下文并创建新的文件共享访问。 在提升的 Windows PowerShell 窗口中运行以下 cmdlet:

        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx

    因此，我们共享，这些是我们运行的 cmdlet:

        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx


现在，它是用户的转弯。 首先，必须安装[远程应用程序客户端](remoteapp-clients.md)用户。 接下来，用户需要从他们的帐户为您创建该 Azure 文件共享驱动器映射并添加其访问文件。 这是如何实现的︰

1. 在远程应用程序客户端来访问已发布的应用程序。 启动 cmd.exe 程序。
2. 运行下面的命令将从您的计算机文件共享的驱动器映射︰

        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>

    如果您**/ 持久性**将参数设置为是，则将在会话间保持映射的驱动器。
1. 现在，启动远程应用程序从该文件资源管理器应用程序。 您想要在该文件共享的共享应用程序中使用的所有访问文件都复制。
![放置在 Azure 的共享访问文件](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
1. 最后，打开 Access，然后打开只需共享的数据库。 您应看到您的数据在 Access 中从云中运行。
![从云中运行的实际访问数据库](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

现在您可以使用访问任何设备-只需确保您安装远程应用程序客户端。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

现在，您已经掌握了创建集合，请尝试创建一个[集合，它使用 Office 365](remoteapp-tutorial-o365anywhere.md)。 也可以创建一个[混合集合](remoteapp-create-hybrid-deployment.md)，可以访问您的本地网络。

<!--Image references-->
 