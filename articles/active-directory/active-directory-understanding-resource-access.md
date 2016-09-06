---
ms.openlocfilehash: 66a3d4e4da8ac9484ee349f2954232c91cd33bf3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
                pageTitle="了解在 Azure 中的资源访问权限？" 
                description="本主题说明如何使用订阅管理员来控制完整 Azure 门户中的访问资源的概念。" 
                services="active-directory" 
                documentationCenter="" 
                authors="markusvi" 
                manager="swadhwa" 
                editor=""/>

<tags 
                ms.service="active-directory" 
                ms.workload="identity" 
                ms.tgt_pltfrm="na" 
                ms.devlang="na" 
                ms.topic="article" 
                ms.date="08/10/2015" 
                ms.author="markusvi"/>


# 了解在 Azure 中的资源访问权限


> [AZURE.NOTE] 本主题说明如何使用订阅管理员来控制完整 Azure 门户中的访问资源的概念。 作为一种替代方法，Azure 预览门户提供[基于角色的访问控制](role-based-access-control-configure.md)，以便可以更精确地管理 Azure 的资源。 

在 2013 年 10 月，Azure 管理门户网站和服务管理 Api 已集成了 Azure Active Directory 以改进管理 Azure 的资源访问权限的用户体验奠定了。 Azure 的 Active Directory 已经提供了很好的功能，如用户管理、 内部目录同步、 多因素身份验证和应用程序的访问控制。 当然，这些也应将提供管理 Azure 资源统一。

从付费的角度启动 Azure 中的访问控制。 通过访问[Azure 帐户中心](https://account.windowsazure.com/subscriptions)访问的 Azure 帐户的所有者是帐户管理员 (AA)。 订阅是计费的容器，但它们也作为一种安全边界︰ 每个订阅的服务管理员 (SA) 可以添加、 删除和修改该订阅中的 Azure 资源使用[Azure 管理门户](https://manage.windowsazure.com/)。 新订阅的默认 SA 是 AA，但是 AA 可以更改 SA 在 Azure 帐户中心。
 
<br><br>![Azure 帐户][1]

还有订阅与目录关联。 目录定义一组用户。 这可能是用户工作或创建目录的学校，或者也可以是外部的用户 （即，Microsoft 帐户）。 订阅是被指派为服务管理员 (SA) 或共同管理员 (CA); 那些目录用户子集的访问唯一的例外是，由于传统原因，Microsoft 帐户 (以前称为 Windows Live™ ID) 可以将指定为 SA 或 CA 而不存在的目录中。

<br><br>![在 Azure 中的访问控制][2]

 
在 Azure 管理门户功能使 SAs 的签名中使用的是 Microsoft 帐户订阅**设置**中的**订阅**页上，通过**编辑目录**命令关联的目录更改。 请注意，此操作意义上该订阅的访问控制。



> [AZURE.NOTE] Azure 管理门户中的**编辑目录**命令不适用于用户登录使用某一工作或学校考虑，因为这些帐户可以登录到其所属的目录。

<br><br>![简单的用户登录流程][3]

在最简单的情况下，组织 （例如 Contoso) 将强制付费和跨一组相同的订阅的访问控制。 也就是说，目录是关联到一个 Azure 帐户所拥有的订阅。 后到 Azure 管理门户的成功登录，用户看到资源 （橙色在上图中所示） 的两个的集合︰


- 目录用户帐户所在的位置 （源或作为外部主体添加）。 请注意，不是用于登录的目录，与此计算中，以便将始终显示您的目录，而不考虑您的登录。

- 订阅，则用于登录的目录相关联，用户就可以访问 （它们在哪里 SA 或 CA） 一部分的资源。


<br><br>![用户与多个订阅和目录][4]


具有跨多个目录订阅的用户能够通过订阅筛选器切换 Azure 管理门户的当前上下文。 在幕后，这会导致另外一种登录到不同的目录，但这是无缝地使用单一登录 (SSO)。 

例如，订阅之间移动资源的操作可能会更加困难的订阅此单个目录视图的结果。 若要执行的资源转移，可能有必要对首次使用在**设置**中的订阅页上的**编辑目录**命令相关联的订阅到相同的目录。 



<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png
测试t
