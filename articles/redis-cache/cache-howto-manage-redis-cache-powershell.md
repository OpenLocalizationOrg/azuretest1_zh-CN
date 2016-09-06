---
ms.openlocfilehash: 8ef1d72e9191aa15753b831c164712967a4d7843
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
 pageTitle="管理 Azure PowerShell Azure 的 Redis 高速缓存 |Microsoft Azure"
 description="了解如何使用 Azure PowerShell 的 Azure Redis 缓存执行管理任务。"
 services="redis-cache"
   documentationCenter=""
   authors="Rick-Anderson"
   manager="wpickett"
   editor="v-lincan"/>

<tags
   ms.service="cache"
   ms.devlang="all"
   ms.topic="article"
   ms.tgt_pltfrm="cache-redis"
   ms.workload="multiple"
   ms.date="08/26/2015"
   ms.author="riande"/>

# 管理与 Azure PowerShell Azure Redis 缓存

本主题演示如何创建、 更新和删除 Azure Redis 高速缓存。

## 先决条件 ##

您可以使用 Windows PowerShell 使用 Azure 资源管理器之前，您需要︰

- Windows PowerShell，3.0 或 4.0 版本。 若要查找 Windows PowerShell 的版本，请键入︰`$PSVersionTable` ，并验证的值`PSVersion`是 3.0 或 4.0。 要安装的兼容版本，请参阅[Windows 管理框架 3.0](http://www.microsoft.com/download/details.aspx?id=34595)或[Windows 管理框架 4.0](http://www.microsoft.com/download/details.aspx?id=40855)。

- Azure PowerShell 版本 0.8.0 或更高版本。 若要安装最新版本，并将其与您 Azure 的订阅，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)。

本教程是为 Windows PowerShell 初学者设计的。 有关 Windows PowerShell 的详细信息，请参阅[Windows PowerShell 入门知识](http://technet.microsoft.com/library/hh857337.aspx)。

要获得在本教程中，您看到任何 cmdlet 的详细的帮助，请使用获取帮助 cmdlet。

    Get-Help <cmdlet-name> -Detailed

例如，要添加 AzureAccount cmdlet 获取帮助，请键入︰

    Get-Help Add-AzureAccount -Detailed

## 一个简单的 Azure PowerShell 脚本 Redis 高速缓存  ##

下面的脚本演示如何创建、 更新和删除 Azure Redis 高速缓存。

        # Azure Redis Cache operations require mode set to AzureResourceManager.
        Switch-AzureMode AzureResourceManager
        $VerbosePreference = "Continue"

            # Create a new cache with date string to make name unique. 
        $cacheName = "MovieCache" + $(Get-Date -Format ('ddhhmm')) 
        $location = "West US"
        $resourceGroupName = "Default-Web-WestUS"

        $movieCache = New-AzureRedisCache -Location $location -Name $cacheName  -ResourceGroupName $resourceGroupName -Size 250MB -Sku Basic

        # Wait until the Cache service is provisioned.

        for ($i = 0; $i -le 60; $i++)
        {
            Start-Sleep -s 30
            $cacheGet = Get-AzureRedisCache -ResourceGroupName $resourceGroupName -Name $cacheName
            if ([string]::Compare("succeeded", $cacheGet[0].ProvisioningState, $True) -eq 0)
            {
                break
            }
            If($i -eq 60)
            {
                exit
            }
        }

        # Update the access keys.

        Write-Verbose "PrimaryKey: $($movieCache.PrimaryKey)"
        New-AzureRedisCacheKey -KeyType "Primary" -Name $cacheName  -ResourceGroupName $resourceGroupName -Force
        $cacheKeys = Get-AzureRedisCacheKey -ResourceGroupName $resourceGroupName  -Name $cacheName
        Write-Verbose "PrimaryKey: $($cacheKeys.PrimaryKey)"

        # Use Set-AzureRedisCache to set Redis cache updatable parameters.
        # Set the memory policy to Least Recently Used.

        Set-AzureRedisCache -MaxMemoryPolicy AllKeysLRU -Name $cacheName -ResourceGroupName $resourceGroupName

        # Delete the cache.

        Remove-AzureRedisCache -Name $movieCache.Name -ResourceGroupName $movieCache.ResourceGroupName  -Force

## 下一步行动

若要了解有关 Windows PowerShell 使用 Azure 的详细信息，请参阅以下资源︰

- [Azure 资源管理器的 Cmdlet](http://go.microsoft.com/fwlink/?LinkID=394765)︰ 了解如何使用 AzureResourceManager 模块中的 cmdlet。
- [使用资源组来管理 Azure 资源](../azure-portal/resource-group-portal.md)︰ 了解如何创建和管理在 Azure 预览门户的资源组。
- [Azure 博客](http://blogs.msdn.com/windowsazure)︰ 了解 Azure 中的新功能。
- [Windows PowerShell 博客](http://blogs.msdn.com/powershell)︰ 了解 Windows PowerShell 中的新功能。
- ["的您好，脚本专家 ！"博客](http://blogs.technet.com/b/heyscriptingguy/)︰ 从 Windows PowerShell 社区获取真实的提示和技巧。
