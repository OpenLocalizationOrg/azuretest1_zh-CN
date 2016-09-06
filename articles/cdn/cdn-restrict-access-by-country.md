---
ms.openlocfilehash: 78db07e9e881563456899492a46a596ad6582031
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="CDN 的限制访问您的内容按国家/地区" 
    description="当用户请求内容，默认情况下时，无论用户在其中做出这样的请求被提供内容。 在某些情况下，您可能需要限制对您的内容按国家/地区的访问。 本主题说明如何使用**国家筛选**功能，以便将服务配置为允许或阻止访问按国家/地区。" 
    services="cdn" 
    documentationCenter=".NET" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cdn" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="juliako"/>

#限制访问您的内容按国家/地区

当用户请求内容，默认情况下时，无论用户在其中做出这样的请求被提供内容。 在某些情况下，您可能需要限制对您的内容按国家/地区的访问。 本主题说明如何使用**国家筛选**功能，以便将服务配置为允许或阻止访问按国家/地区。

>[AZURE.NOTE]一旦配置的设置，将应用于您的订阅中的所有 CDN 终结点。

有关注意事项适用于配置这种类型的限制的信息，请参阅本主题的末尾[注意事项](cdn-restrict-access-by-country.md#considerations)一节。  

![国家筛选](./media/cdn-filtering/cdn-country-filtering.png)


##步骤 1︰ 定义的目录路径

在配置的国家/地区筛选器时，必须指定的允许或拒绝访问的用户的位置的相对路径。 您可以应用您的所有文件的筛选国家"/"或通过指定目录路径中选择文件夹。

示例目录路径筛选器︰

    /                                 
    /Photos/
    /Photos/Strasbourg

##步骤 2︰ 定义的操作︰ 阻止或允许

**块︰**从指定的国家/地区的用户将被拒绝访问资产要求从该递归路径。 如果没有其他国家筛选选项已配置为该位置了，那么所有其他用户将被允许访问。

**允许︰**仅从指定的国家/地区的用户将允许对资产要求从该递归路径的访问。

##第 3 步︰ 定义国家/地区

选择希望阻止或允许的路径的国家/地区。 有关详细信息，请参阅该[国家/地区代码](cdn-country-codes.md)的主题。

例如，阻止 /Photos/Strasbourg/规则将筛选的文件包括︰

    http://az123456.vo.msecnd.net/Photos/Strasbourg/1000.jpg. 
    http://az123456.vo.msecnd.net/Photos/Strasbourg/Cathedral/1000.jpg. 


##国家/地区代码

**国家筛选**功能使用国家/地区代码来定义的国家的允许或阻止对受保护的目录请求。 [本](cdn-country-codes.md)主题中，您将找到国家/地区代码。 如果您指定"欧盟"（欧洲） 或"接入点"（亚太），则将阻止或允许来自该区域任何国家/地区的 IP 地址的子集。 


##<a id="considerations"></a>注意事项

- 它会占用一个小时对贵国筛选配置更改才能生效。
- 此功能不支持通配符字符 (例如，*)。
- 筛选与相对路径相关联的配置的国家/地区将递归地应用于该路径。
- 一个规则可以应用于相同的相对路径 （不能创建指向相同的相对路径的多个国家筛选器。 但是，文件夹可能有多个国家筛选器。 这是由于国家筛选器的递归性质。 换句话说，以前配置文件夹的子文件夹，可以分配不同国家筛选器。
测试
