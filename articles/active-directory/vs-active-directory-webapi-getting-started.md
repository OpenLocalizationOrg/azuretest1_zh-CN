---
ms.openlocfilehash: d08da29955af769e1f7986a6b0969a78e116b593
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="" 
    description="有关快速入门 Azure Active Directory （Web API 项目） 向导的信息" 
    services="active-directory" 
    documentationCenter="" 
    authors="patshea123" 
    manager="douge" 
    editor="tglee"/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/22/2015" 
    ms.author="patshea123"/>

# 要开始使用 Azure Active Directory （Web API 项目）

> [AZURE.SELECTOR]
> - [入门教程](vs-active-directory-webapi-getting-started.md)
> - [发生了什么事](vs-active-directory-webapi-what-happened.md)

##要求验证访问控制器
 
您的项目中的所有控制器被都装饰与**授权**特性。 此特性将要求用户进行身份验证才能访问这些控制器所定义的 Api。 若要允许匿名访问控制器，从控制器中删除此属性。 如果您想要在更精细的级别设置权限，给需要授权而不是将其应用于控制器类的每个方法应用的特性。

[了解更多有关 Azure Active Directory](http://azure.microsoft.com/services/active-directory/)
 
测试
