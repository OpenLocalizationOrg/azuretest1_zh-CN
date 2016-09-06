---
ms.openlocfilehash: a809d4d86304cb54dd9e44db5bce754b05375464
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="安装弹性数据库作业" 
    description="逐步安装弹性作业功能。" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="sidneyh" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/03/2015" 
    ms.author="ddove; sidneyh"/>

# 安装弹性数据库作业概述
**弹性数据库作业**可以安装通过 PowerShell 或 Azure 的入口，虽然您才可以访问用于创建和管理作业使用 PowerShell API，只有在安装 PowerShell 包。 此外，PowerShell Api 提供多得多的功能比门户在此时间点。 关于**弹性数据库作业**的详细信息，请参阅[弹性数据库作业概述](sql-database-elastic-jobs-overview.md)。

如果您已经从现有**弹性数据库池**安装**弹性数据库作业**通过门户网站，最新的 Powershell 预览包括用于将现有安装升级脚本。 强烈建议要充分利用新功能，通过 PowerShell Api 公开将安装升级到最新的**弹性数据库作业**组件。

## 先决条件
* Azure 的订阅。 免费试用版，请参阅[免费试用版](http://azure.microsoft.com/pricing/free-trial/)。
* Azure PowerShell 版本 > = 0.8.16。 安装最新版本 (0.9.5) 通过[Web 平台安装程序](http://go.microsoft.com/fwlink/p/?linkid=320376)。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)。
* [NuGet 命令行实用程序](https://nuget.org/nuget.exe)用于安装弹性数据库作业包。 有关详细信息，请参阅 http://docs.nuget.org/docs/start-here/installing-nuget。

## 下载并导入弹性数据库作业 PowerShell 包
1. 启动 Microsoft Azure PowerShell 命令窗口并定位到您下载 NuGet 命令行实用工具 (nuget.exe) 的目录。

2. 下载并将**弹性数据库作业**包导入到当前目录中使用下面的命令︰

        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease

    **弹性数据库作业**文件放在一个名为**Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** *x.x.xxxx.x*在其中反映的版本编号文件夹中的本地目录。 （包括所需的客户端的.dll） 的 PowerShell cmdlet 都位于**tools\ElasticDatabaseJobs**子目录，PowerShell 脚本来安装、 升级和卸载也驻留在**工具**的子目录中。

3. 导航到工具 Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x 文件夹下的子目录下，键入 cd 工具，例如︰

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4.  执行.\InstallElasticDatabaseJobsCmdlets.ps1 脚本将 ElasticDatabaseJobs 目录复制到 $home\Documents\WindowsPowerShell\Modules。 这将自动导入模块供使用，例如︰

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1 
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## 安装使用 PowerShell 的弹性数据库作业组件
1.  启动 Microsoft Azure PowerShell 命令窗口，然后定位到 \tools 的 Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x 文件夹下的子目录︰ 键入 cd \tools

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2.  执行.\InstallElasticDatabaseJobs.ps1 PowerShell 脚本，并提供为其请求变量的值。 此脚本将创建所述[弹性数据库作业组件和定价](sql-database-elastic-jobs-overview/#components-and-pricing)及适当地使用从属组件将 Azure 云服务配置的组件。

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1 
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

当您运行此命令时打开一个窗口，要求输入**用户名**和**密码**。 这不是 Azure 凭据、 输入的用户名称和要创建的新服务器的管理员凭据的密码。 

在此示例调用提供的参数可以修改为您所需的设置。 下面提供了每个参数的行为的详细信息︰

<table style="width:100%">
  <tr>
    <th>参数</th>
    <th>说明</th>
  </tr>

<tr>
    <td>ResourceGroupName</td>
    <td>提供创建包含新创建的 Azure 组件的 Azure 的资源组名称。 此参数默认为"__ElasticDatabaseJob"。 建议不要更改此值。</td>
    </tr>

</tr>

    <tr>
    <td>ResourceGroupLocation</td>
    <td>提供要用于新创建的 Azure 元件的 Azure 位置。 到美国中部的位置，此参数默认值。</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>提供安装服务工作人员的数目。 此参数默认为 1。 扩展服务，以提供高可用性，可以使用更多的工作人员。 建议用于需要高可用性服务的部署，"2"。</td>
    </tr>

</tr>
    <tr>
    <td>ServiceVmSize</td>
    <td>提供云服务中使用虚拟机大小。 此参数默认为 A0。 参数值的 A0/A1/A2/A3 接受这导致辅助角色，使用 ExtraSmall/小型/中型/大型的大小，分别。 Fo 辅助角色大小，详细信息请参阅[弹性数据库作业组件和定价](sql-database-elastic-jobs-overview/#components-and-pricing)。</td>
</tr>

</tr>
    <tr>
    <td>SqlServerDatabaseSlo</td>
    <td>标准版提供的服务等级目标。 此参数默认为 S0。 接受这导致 SQL Azure 数据库使用各自的 SLO S0/S1/S2/S3 的参数值。 SQL 数据库 Slo 的详细信息，请参阅[弹性数据库作业组件和定价](sql-database-elastic-jobs-overview/#components-and-pricing)。</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorUserName</td>
    <td>提供新创建的 SQL Azure 数据库服务器的管理员用户名。 如果未指定，PowerShell 凭据将打开一个窗口来提示输入凭据。</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorPassword</td>
    <td>新创建的 SQL Azure 数据库服务器提供管理员密码。 如果不提供，PowerShell 凭据将打开一个窗口来提示输入凭据。</td>
</tr>
</table>

对于目标有大量针对大量数据库的并行运行的作业的系统，建议指定参数如︰-ServiceWorkerCount 2-ServiceVmSize A2-SqlServerDatabaseSlo S2。

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## 更新现有的弹性数据库作业组件安装使用 PowerShell

可以扩展和高可用性的现有安装中更新**弹性数据库作业**。 此过程允许将来升级服务代码而无需除去并重新创建管理数据库。 此过程还可相同的版本中修改服务 VM 大小或服务器辅助计数。

要更新的安装虚拟机大小，请使用更新为您选择的值的参数运行以下脚本。

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th>参数</th>
  <th>说明</th>
</tr>

  <tr>
    <td>ResourceGroupName</td>
    <td>标识的弹性数据库作业组件开始安装时使用的 Azure 的资源组名称。 此参数默认为"__ElasticDatabaseJob"。 建议不要更改此值，因为您不必指定该参数。</td>
    </tr>
</tr>

</tr>

  <tr>
    <td>ServiceWorkerCount</td>
    <td>提供安装服务工作人员的数目。  此参数默认为 1。  扩展服务，以提供高可用性，可以使用更多的工作人员。  建议用于需要高可用性服务的部署，"2"。</td>
</tr>

</tr>

    <tr>
    <td>ServiceVmSize</td>
    <td>提供云服务中使用虚拟机大小。 此参数默认为 A0。 参数值的 A0/A1/A2/A3 接受这导致辅助角色，使用 ExtraSmall/小型/中型/大型的大小，分别。 Fo 辅助角色大小，详细信息请参阅[弹性数据库作业组件和定价](sql-database-elastic-jobs-overview/#components-and-pricing)。</td>
</tr>

</table>

## 使用门户弹性数据库作业组件安装

一旦[创建弹性数据库池](sql-database-elastic-pool-portal.md)，您可以安装**弹性数据库作业**组件能够弹性数据库池中的每个数据库管理任务的执行。 与使用**弹性数据库作业**PowerShell Api 时门户界面是当前限制为仅执行对现有池。


**估计完成时间︰** 10 分钟。

1. 从弹性数据库池通过[Azure 预览门户](https://ms.portal.azure.com/#)的仪表板视图，请单击**创建作业**。
2. 如果您第一次创建一个作业，则必须通过单击**预览条款**安装**弹性数据库作业**。 
3. 单击复选框来接受的条款。
4. 在"安装服务"视图中，单击**作业凭据**。

    ![安装服务][1]

5. 键入用户名和密码的数据库管理员。 作为安装的一部分，创建新的 SQL Azure 数据库服务器。 在此新服务器中，一个新的数据库，称为管理数据库中，创建和使用包含弹性数据库作业的元数据。 用户名称和密码在此处创建用于登录到管理数据库。 一个单独的凭据用于对池内数据库的脚本执行。

    ![创建用户名和密码][2]

6. 单击确定按钮。 组件会为您创建新的[资源组](../resource-group-portal.md)中的几分钟时间。 新的资源组被固定到开始板，如下所示。 一旦所有创建的组中创建、 弹性数据库作业 （云服务、 SQL 数据库、 服务总线和存储）。

    ![在开始板中的资源组][3]

7. 如果您尝试创建或管理作业时弹性数据库作业安装提供**凭据**时，您将看到下面的消息。 

    ![仍在进行的部署][4]

如果必须卸载，则删除该资源组。 了解[如何卸载弹性数据库作业组件](sql-database-elastic-jobs-uninstall.md)。

## 下一步行动

确保具有适当权限的脚本执行凭据创建的组中，每个数据库的详细信息，请参阅[如何将用户添加到所有数据库中我的数据库的用户组](sql-database-elastic-jobs-add-logins-to-dbs.md)。 请参阅[创建和管理弹性数据库作业](sql-database-elastic-jobs-create-and-manage.md)开始。

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/incomplete.png
 