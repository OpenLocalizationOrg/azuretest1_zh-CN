---
ms.openlocfilehash: 24d1f33f6c94dd8245643c5310f4efbdb9286891
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何卸载弹性数据库作业工具" 
    description="如何卸载弹性数据库作业工具" 
    services="sql-database" 
    documentationCenter="" 
    manager="jeffreyg" 
    authors="sidneyh" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/09/2015" 
    ms.author="ddove; sidneyh"/>

#卸载数据库弹性作业组件
可以使用门户或 PowerShell 卸载**作业弹性数据库**组件。

##卸载数据库弹性作业组件使用 Azure 门户

1. 打开[Azure 的门户](https://ms.portal.azure.com/)。
2. 导航到包含**弹性数据库作业**组件，即弹性数据库作业组件已安装在其中的订阅的订阅。
3. 单击**浏览**，然后单击**资源组**。
4. 选中名为"__ElasticDatabaseJob"的资源组。
5. 删除资源组。

##卸载使用 PowerShell 的弹性数据库作业组件

1.  启动 Microsoft Azure PowerShell 命令窗口并定位到 Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x 文件夹下的工具子目录︰ 键入 cd 工具

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2.  执行.\UninstallElasticDatabaseJobs.ps1 PowerShell 脚本。

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1

或简单，执行下面的脚本中，假定默认值使用组件的安装位置︰

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager
        
        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }
        
        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## 下一步行动

要重新安装弹性数据库作业，请参阅[安装弹性数据库作业服务](sql-database-elastic-jobs-service-installation.md)

弹性数据库作业的概览，请参阅[弹性数据库作业概述](sql-database-elastic-jobs-overview.md)。

<!--Image references-->
[1]: ./media/sql-database-elastic-job-uninstall/
 
