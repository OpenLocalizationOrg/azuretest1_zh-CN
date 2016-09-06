---
ms.openlocfilehash: efff6eec21d611fb18083dc99b2bd27b37614f56
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Azure Active Directory 组管理对资源的访问 |Microsoft Azure" 
    description="解释如何使用 Azure AD 中访问管理组的主题。" 
    services="active-directory" 
    documentationCenter="" 
    authors="femila" 
    manager="swadhwa" 
    editor=""
    tags="azure-classic-portal"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/14/2015" 
    ms.author="femila"/>


# 使用 Azure Active Directory 组管理对资源的访问

Azure 的 Active Directory 是全面的身份和访问管理解决方案，它提供一套可靠的能力，以管理对内部和云应用程序和资源，包括 Microsoft Office 365 和 Microsoft SaaS 应用程序的世界等的在线服务的访问。


> [AZURE.NOTE] 若要使用 Azure Active Directory，您需要 Azure 帐户。 如果您没有帐户，则可以[注册一个免费的 Azure 帐户](http://azure.microsoft.com/pricing/free-trial/)。


在 Azure Active Directory 中的主要功能之一是能够管理对资源的访问。 这些资源可以是一部分的目录，如管理通过角色中的目录或目录，如 SaaS 应用程序、 Azure 服务和 SharePoint 网站或对内部资源与外部资源的对象的权限的情况。
有 4 种方式可将用户分配到的资源的访问权限︰


1\.直接赋值

用户可以直接对资源分配由该资源的所有者。

2\.组成员资格

一个组可以被分配给资源由资源的所有者，并通过这种方式，授予访问资源的组的成员。 然后可以按组的所有者管理组的成员身份。 有效地资源所有者委托将用户分配给组的所有者对其资源的权限。

3\.基于规则

资源所有者可以使用规则来表示应将分配给的用户访问资源。 规则的结果取决于该规则和它们的值中使用的特定用户的属性，这样，资源所有者有效委托管理权威来源于其资源对规则中使用的属性的访问权限。 请注意，资源所有者仍管理本身的规则，确定哪些属性和值提供对其资源的访问。

4\.外部机构

对资源的访问权限被从外部源，例如从权威的来源，例如在部署目录或如工作日 SaaS 应用程序同步的组。 资源所有者分配的组提供对资源的访问和外部源管理组的成员。

  ![](./media/active-directory-access-management-groups/access-management-overview.png)


###观看视频，解释访问管理

您可以监视的详细解释此[这里](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups)的短视频。

##在 Azure Active Directory 访问管理是如何工作的？
该中心的 Azure 活动目录的访问权限管理解决方案是安全组。 使用安全组来管理对资源的访问是众所周知的范例，从而提供了一种灵活且易于理解的方法，为预期的用户组提供对资源的访问。 资源所有者 （或目录管理员） 可以分配一组提供直接到他们所拥有的资源的访问。 组的成员将获得访问权限，以及资源所有者可以委派管理给别人--例如，部门经理或帮助台管理员组的成员列表的权限。

![](./media/active-directory-access-management-groups/active-directory-access-management-works.png)
组的所有者还可以使该组可用于自助服务请求。 这样，最终用户可以搜索并找到组和发出请求加入，有效地寻求通过组访问进行管理的资源的权限。 组的所有者可以对组进行设置，以便加入请求自动批准或由所有者组的批准。 当用户请求加入一组，加入请求将被转发到组的所有者。 如果所有者之一批准该请求，发出请求的用户会收到通知，用户加入到组。 如果一个物主拒绝该请求，发出请求的用户通知，但未加入到组。


## 访问管理入门
准备好开始了吗？ 您应该尝试一些可以使用 Azure AD 组完成的基本任务。 使用这些功能以便为您的组织中不同资源的专用的访问不同组的人员。 下面列出了一份基本的第一步。


* [创建一个简单的规则来配置动态组的成员身份](active-directory-accessmanagement-simplerulegroup.md)

* [使用组来管理对 SaaS 应用程序的访问](active-directory-accessmanagement-group-saasapps.md)

* [提供一组用于最终用户自助服务](active-directory-accessmanagement-self-service-group-management.md)

* [同步到 Azure 使用 Azure AD 连接后端组](active-directory-aadconnect.md)

* [管理向组的所有者](active-directory-accessmanagement-managing-group-owners.md) 


## 下一步行动来访问管理
既然已经理解访问管理的基本知识，下面是一些其他高级的功能 Azure Active Directory 中可用来管理您的应用程序和资源的访问。

* [使用一个简单的规则来创建一组](active-directory-accessmanagement-simplerulegroup.md) 

* [使用属性可创建高级的规则](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [在 Azure Active Directory 管理安全组](active-directory-accessmanagement-manage-groups.md)

* [设置 Azure Active Directory 中的专用组](active-directory-accessmanagement-dedicated-groups.md)


## 了解更多信息
下面是一些将 Azure Active Directory 提供一些附加信息的主题 

* [什么是 Azure 活动目录？](active-directory-whatis.md)

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)

* [组的图形 API 参考](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

测试
