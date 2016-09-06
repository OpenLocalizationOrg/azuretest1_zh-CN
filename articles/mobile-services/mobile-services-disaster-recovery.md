---
ms.openlocfilehash: 174c02ab3ce1f598a0c62d2d7475bcf4204377bf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="恢复您的移动服务在发生灾难时 |Microsoft Azure"
    description="了解如何您的移动服务在发生灾难时恢复。"
    services="mobile-services"
    documentationCenter=""
    authors="christopheranderson"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/08/2015"
    ms.author="christopheranderson"/>

# 您的移动服务在发生灾难时恢复

当使用 Azure 移动服务来部署应用程序时，可以使用其内置功能，以确保业务连续性，在可用性问题，例如服务器故障、 网络中断、 数据丢失和广泛设施损失发生。 通过部署您的应用程序使用 Azure 移动服务正在利用许多容错能力和基础结构需要设计、 实施和管理，如果您要部署一个传统的内部部署版本解决方案的能力。 Azure 缓解大部分潜在的故障成本的一小部分。

## <a name="prepare"></a> 为可能发生的灾难做好准备

为便于恢复可用性问题时，您可以事先做好准备为它︰

+ **备份在 Azure 的移动数据服务 SQL 数据库**
  您的移动服务的应用程序数据存储在 SQL Azure 数据库。 我们建议您备份该[SQL 数据库业务连续性指南]中的规定。
+ **重新启动您的移动服务的脚本**
  建议您将您的移动服务的脚本存储在源代码控制系统如[团队基础服务]或 [GitHub]，并不能仅依靠中移动服务本身的副本。 您可以下载 Azure 的门户网站，通过脚本使用移动服务的[源控件的功能]，或[使用 Azure CLI]。 请特别注意到标记为门户，在"预览"，并不能保证这些脚本的恢复以及可能需要从自己的原始源代码管理恢复它们的功能。
+ **预留的辅助移动服务**
  在您的移动服务可用性问题，您可能需要重新部署到一个备用的 Azure 区域。 为确保产能可用 （例如在极少数情况下，如整个地区的损失），我们建议您可选区域中创建移动的辅助服务，将其模式设置相同或高于主服务的模式。 (如果您的主要服务是在基本模式下，可以进行辅助服务或者基本或标准。 但如果主要是标准，再辅助也必须为标准。

## <a name="watch"></a>观察有出现问题的迹象

这种情况下表示的问题，可能需要恢复操作︰

+ 应用程序连接到您的移动服务不能与之通信的时间较长。
+ 移动服务状态将显示为**不正常**的[Azure 的门户]。
+ **不正常**的横幅出现在您的移动服务在 Azure 的门户中，每个选项卡的顶部，并管理操作产生错误消息。
+ [Azure 服务仪表板]指示一个可用性问题。

## <a name="recover"></a>从灾难中恢复

出现问题时，使用服务面板获取指南和更新。

如果提示您通过服务面板，执行以下步骤来将您的移动服务还原到备用的 Azure 区域中运行状态。 如果您预先创建的辅助服务，其容量将用于还原的主要服务。 由于 URL 和主要服务的应用程序键恢复后保持不变，您不需要更新任何引用它的应用程序。

停机后恢复您的移动服务︰

1. 在 Azure 的门户中，请确保您的服务的状态，被报告为**不正常**。

2. 如果您已预留移动的辅助服务，您可以跳过此步骤。

   如果已经没有保留的辅助的移动服务，则创建一个现在在 Azure 的另一个区域。 它的缩放模式一样设置作为您的主要服务模式。

3. 配置 Azure 命令行界面 (Azure CLI) 来处理您的订阅[使用 Azure CLI 自动移动服务]中所述。

4. 现在，您可以使用辅助服务恢复您主要的一个。

    > [AZURE.IMPORTANT] 迁移文件，除了迁移命令还会更新主要服务以指向您的辅助服务，以便客户端应用程序不需要更新的主机名。 但是，它将多达 30 分钟的主机名解析为新的服务。 鉴于此，建议在灾难恢复情形中只使用迁移命令。

    > [AZURE.IMPORTANT] 在此步骤中执行命令时，辅助服务被删除，以便其容量可用于恢复的主要服务。 我们建议您备份您的脚本和设置之前运行该命令，如果您想要保留它们。

    准备就绪之后，执行以下命令︰

        azure mobile migrate PrimaryService SecondaryService
        info:    Executing command mobile migrate
        Warning: this action will use the capacity from the mobile service 'SecondaryService' and delete it and the host name for 'PrimaryService' may not resolve for up to 30 minutes. Do you want to migrate the mobile service 'PrimaryService'? [y/n]: y
        + Performing migration
        + Migration with id '0123456789abcdef0123456789abcdef' started. The migration may take several minutes to complete.
        + Cleaning up
        info:    Migration complete. It may take 30 minutes for DNS to resolve to the migrated site.
        info:    mobile migrate command OK

    > [AZURE.NOTE] 它可能需要几分钟后在命令完成之前您可以看到在门户中所做的更改。

5. 验证正确已通过与您在源代码管理中的原始文件进行比较来恢复所有脚本。 在大多数情况下，脚本将自动恢复丢失数据，但如果发现差异时，您可以手动恢复该脚本。

6. 请确保您已恢复的服务进行通信与 SQL Azure 数据库。 Recover 命令恢复移动的服务，但会保留原始数据库的连接。 如果主 Azure 地区问题也会影响数据库，恢复的服务可能仍没有正常运行。 Azure 服务仪表板可用于检查给定区域的数据库状态。 如果不使用原始数据库，您可以恢复它︰
    + 恢复到您刚刚恢复您的移动服务，Azure 地区 SQL Azure 数据库， [SQL 数据库业务连续性指南]中所述。
    + 在 Azure 门户，您的移动服务的**"配置"**选项卡上选择"更改数据库"，然后选择新恢复的数据库。

7. 您的移动服务现在位于不同的物理位置。 您将需要更新发布和/或 git 的凭据，以便更新您正在运行的站点。
    + 如果您使用**.NET 后端**，您的发布配置文件再次设置，[发布您的移动服务](mobile-services-dotnet-backend-windows-store-dotnet-get-started/#publish-your-mobile-service)中所述。 这将更新为指向新的服务位置您发布的详细信息。
    + 如果您正在使用**Javascript 后端**和管理门户网站与您的服务时，您不需要采取任何额外措施。
    + 如果您使用**Javascript 后端**并管理您的服务与节点，更新您的 git 远程以指向新的存储库。 若要执行此操作，请从您 git 远程删除.git 文件路径︰

        1. 查找当前的起始地址远程︰ git 远程 v 原点 https://myservice.scm.azure-mobile.net/myservice.git （读取） 原点 https://myservice.scm.azure-mobile.net/myservice.git （强制）
        3. 更新远程使用相同的 url，但不包含最终.git 文件路径︰ git 远程设置 url 源 https://myservice.scm.azure-mobile.net
        4. 拉从原点以验证它正常工作。

现在您应该在您的移动服务已恢复到一个新的 Azure 区域，现在正在接受来自使用其原始 URL 存储应用程序的通信状态。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[SQL 数据库业务连续性指南]: http://msdn.microsoft.com/library/windowsazure/hh852669.aspx
[团队的基础服务]: http://tfs.visualstudio.com/

[源代码控制功能]: http://www.windowsazure.com/develop/mobile/tutorials/store-scripts-in-source-control/
[使用 Azure CLI]: http://www.windowsazure.com/develop/mobile/tutorials/command-line-administration/
[Azure 门户]: http://manage.windowsazure.com/
[Azure 服务仪表板]: http://www.windowsazure.com/support/service-dashboard/
[自动化与 Azure CLI 的移动服务]: http://www.windowsazure.com/develop/mobile/tutorials/command-line-administration/
 
测试
