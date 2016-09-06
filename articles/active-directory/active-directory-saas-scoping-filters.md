---
ms.openlocfilehash: 5e3ef76ea21a1e4b3168bb21f337ae6f203be099
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="基于属性的应用程序资源调配与范围筛选器"
    description="了解如何使用作用域筛选器来阻止支持自动化的用户提供从如果对象不能满足您的业务需求实际配置的应用程序中的对象。"
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
    ms.date="07/27/2015"
    ms.author="markusvi"/>


# 基于属性的应用程序资源调配与范围筛选器

本节的目的是解释如何使用作用域筛选器来定义将确定哪些用户将会提供给应用程序的基于属性的规则。





## 子句和作用域的组


![作用域筛选器][1] 




作用域筛选器定义一个或多个**作用域的组**，其中每个包含一个或多个**子句**。 若要查看特定的作用域组的子句，请单击组名称左侧的箭头来展开。

一个**子句**确定允许哪些用户通过该作用域筛选器，通过计算每个用户的属性。 例如，您可能必须要求用户的状态属性都等于纽约，这意味着您的纽约用户，将提供到应用程序中的一个子句。

![作用域的组名][2] 



每个**作用域的组**启动一个必需的**子句**，在上面的抓图中所示。 此子句只是规定，在计算您的作用域筛选器之前，用户首先必须分配给应用程序。 不能删除或修改此子句。

可以通过按适当的按钮来添加新子句或新作用域的组。 您可以通过编辑其**作用域的组名称**属性为每个作用域的组指定名称。





## 作用域筛选器的计算方式

在资源调配过程中，我们测试了作用域筛选器对每个分配的用户以确定该用户是否值得访问该应用程序。 您可以认为必须使用户能够避免进行筛选传递测试为每个子句。 

如果您有多个定义的作用域组，每个用户必须通过至少一个才能访问该应用程序。 在每个作用域组中，但是，用户必须通过每个单个子句才能通过该特定的作用域的组。 

换句话说，您可以将为作用域组或连接在一起，并为其中的子句可以看作与连接在一起。例如，请考虑下面的作用域筛选器︰


![作用域的组名][2]  


根据此作用域筛选器，用户必须满足以下条件，才能设置︰

1. 他们必须将分配给应用程序。

2. 他们必须在工程部工作

3. 他们必须工作在旧金山或加拿大。


[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png

测试
