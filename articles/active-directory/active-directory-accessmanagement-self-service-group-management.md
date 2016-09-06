---
ms.openlocfilehash: 6f32205732c80272a71d8639f8b3789025e49ea7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="设置 Azure 广告为自助服务应用程序访问管理 |Microsoft Azure"
    description="解释如何在 Azure AD 管理组的主题。"
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
    ms.date="07/13/2015"
    ms.author="femila"/>

#设置自助服务应用程序访问管理 Azure 广告

自助服务组管理使用户能够创建和管理安全组在 Microsoft Azure 活动目录 (AD)，并向用户提供对请求安全组成员，随后可以批准或拒绝的组的所有者身份的可能性。 通过使用自助服务组管理功能，可以了解业务上下文的成员资格的人员到委派组成员身份的日常控制。

自助服务组管理当前组成的两个基本方案︰ 委派组管理和自助服务组管理。


- **委派组管理**需要管理员为管理访问 SaaS 应用程序正在使用她的公司为例。 管理这些访问权限变得比较麻烦，所以此管理员要求企业所有者创建新组。 管理员现在分配为业务所有者只是创建一个新组的应用程序的访问，并将当前有权访问应用程序到此组的所有人员。 企业所有者然后可以添加更多的用户和应用程序时间以后到自动提供了这些用户。 企业所有者就不必等待管理员进行的工作，但可以管理用户的访问自己他。 管理员可以执行相同的操作，对于不同的业务组和企业所有者的行政助理，该行政助理不能触摸或请参阅各自的用户现在可以管理为他们的用户访问。 管理员仍可以看到所有用户有权访问的应用程序和块如果所需的访问权限。


- **自助服务组合管理**的采用该示例的两个用户同时 SharePoint Online 网站他们独立设置，但他们确实想轻松地为其他人提供团队访问。 因此他们在 Azure 广告，创建一个组并在 SharePoint Online 中每个选取同一组提供对站点的访问。 当有人想访问时，请求访问面板中，从和批准之后他们有权两个 SharePoint Online 网站自动。 后面有一种决定访问他的网站中的所有人还应都获取特定 SaaS 应用程序的访问。 他询问此 SaaS 应用程序，添加此应用程序对其网站的访问权限的管理员。 从此以后，他所批准的任何请求将授予访问权限，两个 SharePoint Online 网站以及对此的 SaaS 应用程序。



##提供一组用于最终用户自助服务

在 Azure 管理门户中，在配置选项卡上，设置为启用委派组管理交换机，然后设置用户可以创建组切换到已启用。

当**用户可以创建组**开关设置为**启用**时，您的目录中的所有用户都允许创建新的安全组，并添加到这些组的成员。 请注意，这些新组将还显示在所有其他用户访问权面板和其他用户可以创建组策略设置，这样如果加入这些组的请求。 如果此开关设置为禁用，用户不能创建组并不能更改他们所拥有的但它们仍可以管理这些组的成员资格并批准从其他用户的请求加入它们的组的现有组中。

您还可以使用的用户可以使用安全组开关自助服务来实现用户自助服务组管理功能对更细粒度访问控制的。 当用户可以创建组开关设置为启用，允许在您的目录中的所有用户创建新的安全组，并添加到这些组的成员。 通过还设置可以使用的用户自助服务安全组切换到部分，您将限制到只有有限的一组用户的安全组管理。 如果此开关设置为一些，在您的目录中创建名为 SSGMSecurityGroupsUsers 的组，谁做此组的成员的用户可以创建新的安全组，然后将成员添加到您的目录中的这些组。 通过设置可以使用的用户自助服务安全组开关为 All，则所有用户都启用目录以创建新的安全组中。

您还可以使用的组，可以使用自助服务安全组字段 （默认为 SSGMSecurityGroupsUsers 来指定自己自定义的名称将保留所有用户能够使用自助服务并在您的目录中创建新的安全组的组的组。

下面是一些将 Azure Active Directory 提供一些附加信息的主题

* [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)

* [什么是 Azure 活动目录？](active-directory-whatis.md)

* [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)

测试
