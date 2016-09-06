---
ms.openlocfilehash: bfbd928c5d5ad1706b2a3b23876a7c1923d1538c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="如何使用 Azure RemoteApp Office 365 订阅"
    description="了解如何使用 Office 365 订阅在 Azure 远程应用程序共享 Office 应用程序。"
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2015" 
    ms.author="elizapo" />



# 如何使用 Azure RemoteApp Office 365 订阅

您知道，您可以使用现有 Office 365 订阅在 Azure RemoteApp 共享云从 Office 应用程序吗？ 阅读您的 Office 365 + Azure RemoteApp 选项的信息，充分利用您的订阅进行包括有关 Office 365 的帮助您的文章链接。

## 可以使用 Office 365 订在 Azure 远程应用程序中运行 Office 应用程序？

是的！ 事实上，使用 Office 365 订阅是使 Office 应用程序到 Azure RemoteApp 的唯一办法。

有关 Office 365 订阅其优点是它允许您在很多不同的平台和环境，包括 Azure 的云上使用相同的用户许可证。 在 Azure 远程应用程序中使用 Office 应用程序时不需要购买其他的许可证或任何特殊方式配置您现有的许可证。 只需有包括[Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx)Office 365 订阅。

Office 365 ProPlus 使[共享的计算机激活](https://technet.microsoft.com/library/Dn782860.aspx)-此功能允许在虚拟办公室的临时的基于用户的激活和类似 Azure 远程应用程序 （和远程桌面服务） 的云环境。

哪些 Office 365 计划包括 Office 365 ProPlus？ 签出[每个计划内的服务可用性](https://technet.microsoft.com/library/office-365-plan-options.aspx)表。 请注意，并非所有的计划包括 Office 365 ProPlus （例如，Office 365 业务计划）。 如果没有为您的计划，考虑到计划的功能 （例如，Office 365 教育 E3） 升级。

## 好，那是我的办公室 365 使用 Azure RemoteApp 的 ProPlus 许可证？

Office 365 ProPlus 为每个用户许可证允许单个用户激活 Office 应用程序在最多 5 机加上平板电脑和手机。 每个激活注册用户直到他们停用设备上的办公室。 （用户可以管理他们的设备在[Office 365 门户](https://portal.office365.com/)）。

只要您 （管理员） 将 Office 365 ProPlus 许可证分配给您的用户，他们可以使用 Office 的个人设备上，也可通过 Azure 远程应用程序集合。

## 可以使用 Office 365 和 Azure RemoteApp 的 Office 应用程序？

Office 365 ProPlus 订购可用于共享 Office 2013 和 Office 2016 （后它被释放）。 Azure 的远程应用程序不支持早期版本的 Office。

## 有关 Visio Pro 或项目什么专业？

Office 365 ProPlus 图像包含在您的远程应用程序订阅包括 Visio Pro 和专业项目。 但不能用于您的 Office 365 ProPlus 订阅激活这些程序-它们各有自己的许可证。 您可以在[Office 365 门户](https://portal.office365.com/)中激活它们。 

您没有授权这些程序，如果您不想使用它们。 只需激活您想要使用-并跳过其他的程序。 将在图像中，看到他们，但不是能使用它们。 

## 我如何开始使用 Office 365 和 Azure 远程应用程序？

现在，您了解 Office 365 授权的详细信息，让我们帮助您开始使用 Azure RemoteApp-很容易︰

创建 Azure RemoteApp 集合时，使用**Office 365 ProPlus （所需的订阅）**的图像。

![与 Office 365 专业人员加上 azure RemoteApp 图像](./media/remoteapp-officesubscription/remoteapp-officeimage.png)


此映像包含最新版本的 Windows 服务器和 Office 365 ProPlus。 您只需配置集合 （包括发布的应用程序），您的用户后登录到 Azure 远程应用程序 （通过使用其远程应用程序客户端） 并为其 Office 365 提供凭据提供任何 Office 应用程序。 许可证自动发送未设置任何或管理要求。

## 可以使用 Office 365 ProPlus 创建的自定义图像？

您可以为集合，其中包含 Office 365 ProPlus 创建的自定义图像。 有 2 个选项-使用我们提供的 Azure 库图像或可以创建您自己的自定义图像，并安装那里 Office 365 ProPlus。

### 使用 Azure 的插图

部署 Office 365 ProPlus 集合的最简便方法是[使用 Azure 库图像中的一个启动](remoteapp-image-on-azurevm.md)包括在 Azure RemoteApp 订阅。 请确保您选择的**预安装 Windows 服务器远程桌面会话主机与 Office 365 ProPlus**图像。 然后，将所需的任何其他应用程序安装在该图像，并且您是好去。

### 使用自定义图像

您始终可以创建自定义映像-可以创建[Azure 虚拟机](remoteapp-image-on-azurevm.md)或[创建本地映像](remoteapp-create-custom-image.md)并将其上载到 Azure。 在任一情况下，请确保安装 Office 365 ProPlus 使用共享的计算机激活节点。 使用[Office 部署工具](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx)，然后按照[说明](https://technet.microsoft.com/library/Dn782858.aspx)进行安装。  

### 禁用自动更新的 Office 365 ProPlus 在您的自定义图像的重要

自定义映像用于通过 Azure RemoteApp 作为模板从用户增加根据需要添加更多资源。 要防止延迟和连接问题，请禁用自动图像中的 Office 更新。 如果不这样做，然后用该模板创建的每个资源在启动时将自动更新。 相反，使用标准的 Azure RemoteApp 过程用于更新您的自定义图像。 通过这种方式更新模板图像上一次的 Office 应用程序，然后让 Azure RemoteApp 负责给您的用户获取更新。

若要禁用自动更新，请向 Office 部署工具配置文件中添加以下︰

        <Updates Enabled="FALSE" />

因此，现在您的配置文件应包含以下行︰
    
        <Display Level="NONE" AcceptEULA="TRUE" />
        <Propery Name="SharedComputerLicensing" Value="1" />
        <Updated Enabled="FALSE" />

## 那么，如何用 Office 365 ProPlus 更新图像？

有很多原因来更新您的集合中的图像。 这里有几个︰

- 获取最新的 Windows 更新 
- 获取最新的 Office 365 ProPlus 应用程序更新
- 更新您的自定义应用程序
- 更改图像本身的其他配置设置

对于端到端步骤更新您的集合使用更新的映像，转到[此处](remoteapp-update.md)。 但有关如何更新图像和 Office 365 ProPlus，检查出下面的信息。

您有两个选项可用于更新您的映像-更换您的图像，用一个完整的全新或手动更新您现有的图像。

### 您的图像替换为最新的 Azure 库图像 + 添加自定义项
使用此选项，您可以让 Microsoft Windows 服务器和 Office 365 ProPlus 更新处理。 而不是更新现有的映像，您将创建基于最新的插图完全新的映像。 然后，重复以前自定义映像-安装自定义应用程序，任何步骤修改映像配置等。

库图像会定期更新，因此您可以高枕无忧，了解 Windows Server 和 Office 365 ProPlus 应用程序最新版本。 当然，平衡是需要应用自定义设置，每次获取一个新的映像。 您可以创建脚本以自动执行您的自定义设置。

### 手动更新现有的映像

使用此选项，您可以使用标准的 Windows 工具将更新应用到图像。 对于 Office 365 ProPlus 使用 Office 部署工具下载并安装最新的更新或版本的 Office 365 ProPlus。

> [AZURE.IMPORTANT] 请记住-禁用 Office 365 ProPlus 自动更新。

需要使用 Office 部署工具进行更新的详细信息？

- [通过使用 Office 部署工具部署 Office 365 的产品即点即用](https://technet.microsoft.com/library/JJ219423.aspx)
- [的部署和更新 Office 365 ProPlus 使用 Office 部署工具](https://channel9.msdn.com/Events/Ignite/2015/BRK3168)（视频）
- [配置更新设置 Office 365 ProPlus](https://technet.microsoft.com/library/dn761708.aspx)
