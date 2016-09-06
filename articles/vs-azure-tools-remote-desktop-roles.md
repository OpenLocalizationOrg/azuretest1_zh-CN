---
ms.openlocfilehash: 7edcab5cd4944b774c48966955ed39027c3ebb36
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="远程桌面使用 Azure 角色"
   description="远程桌面使用 Azure 角色"
   services="visual-studio-online"
   documentationCenter="na"
   authors="kempb"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2015"
   ms.author="kempb" />

# 远程桌面使用 Azure 角色

通过使用 Azure SDK 和远程桌面服务，您可以访问 Azure 角色和通过 Azure 托管的虚拟机。 在 Visual Studio 中，您可以从 Azure 项目配置远程桌面服务。 要启用远程桌面服务，必须创建一个工作项目包含一个或多个角色，然后将其发布到 Azure。

>[AZURE.IMPORTANT] 应访问疑难解答或仅开发 Azure 角色。 每个虚拟机的目的是 Azure 应用程序，以运行其他客户端应用程序中运行一个特定的角色。 如果您想要使用 Azure 托管虚拟机，您可以出于任何目的使用，请参阅从服务器资源管理器访问 Azure 的虚拟机。

## 若要启用并使用远程桌面 Azure 角色

1. 在解决方案资源管理器中打开您的项目的快捷菜单，然后选择**发布**。

    将出现**发布 Azure 应用程序**向导。

    ![发布命令为云服务项目](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)

1. 在**Microsoft Azure 发布设置**向导页的底部，选择所有角色复选框用于**启用远程桌面**。 

    出现**远程桌面配置**对话框。

1. 在**远程桌面配置**对话框的底部，选择**更多选项**按钮。 
 
    这将显示下拉列表框，您可以创建或选择一个证书，以便通过远程桌面连接时，您可以加密的凭据信息。

1. 在下拉列表中，选择**<Create>**，或选择一个现有列表中的。 

    如果您选择一个现有的证书，请跳过以下步骤。

    >[AZURE.NOTE] 所需的远程桌面连接的证书是不同于您用于其他 Azure 操作证书。 远程访问证书必须有一个私钥。

    显示**创建证书**对话框。

    1. 提供的新证书的好记名称，然后选择**确定**按钮。 新的证书将显示在下拉列表框中。

    1. 在**远程桌面配置**对话框中提供的用户名和密码。
    
        您不能使用现有的帐户。 不指定管理员为新帐户的用户名。

        >[AZURE.NOTE] 如果密码不符合复杂性要求，在密码文本框旁边将显示一个红色图标。 密码必须包含大写字母、 小写字母和数字或符号。

    1. 选择一个日期，该帐户将过期及以后的远程桌面连接将被阻止。

    1. 提供了所有必需的信息后，请选择**确定**按钮。
    
        .Cscfg 和.csdef 文件添加启用远程 Access Services 的几个设置。

1. 在**Microsoft Azure 发布设置**向导中，选择**确定**按钮时就可以发布您的云服务。

    如果你不准备发布，请选择**取消**按钮。 配置设置被保存，并且您可以稍后发布您的云服务。

## 通过使用远程桌面连接到 Azure 角色

发布在 Azure 云服务之后，可以使用服务器资源管理器登录到虚拟机的 Azure 的主机。 

1. 在服务器资源管理器中，展开**Azure**节点，然后展开云服务和其角色显示实例的列表中的一个节点。

1. 打开快捷菜单实例节点，然后选择**使用远程桌面连接**。

    ![通过远程桌面连接](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)

1. 输入的用户名称和您以前创建的密码。 您现在将登录到远程会话。



测试
