---
ms.openlocfilehash: a88b6c0f96d76752f7f75ae700db4980bad3d291
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Web 应用程序概述"
    description="了解有关应用程序服务 Web 应用程序的详细信息"
    services="app-service\web"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/17/2015"
    ms.author="jaime.espinosa"/>


#Web 应用程序概述

[应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)是一个完全托管平台的专业开发人员带来了丰富的功能与 web、 移动和集成方案。 快速创建和部署任务关键的 web 应用程序与您的业务使用 Azure 应用程序服务进行扩展。

利用[应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)能够使用的语言和框架知道和依赖、 部署快速到 Azure 云应用程序和不断改进您的代码，而无需再担心基础结构。

![Web 市场](./media/app-service-web-overview/marketplace.png)

## 不只是网站##

现代企业在以往任何时候都更为复杂的方式与客户进行交互。 所有类型的公司认为他们公司的 Web 平台作为他们的业务，其业务计划中的主要组件的关键组成部分。 为了适应这种重要性，企业正在寻找一个平台，将为它们提供的灵活性、 安全性和可扩展性。此外，它们还需要能够链接到其现有的业务系统，将无法在全球快速部署新的代码和启动实例。 Azure 应用程序服务和 Web 应用程序，组织可以快速经济地户满意客户。

## 为什么 Web 应用程序？ ##

Azure 应用程序服务 Web 应用程序是一个完全托管的平台使您能够生成、 部署和扩展企业级 web 应用程序，以秒为单位。 重点放在应用程序代码中，并让 Azure 小心来扩展和安全地运行它为您的基础结构。 Web 应用程序是︰

- **熟悉和快速**-使用您喜欢的语言、 框架和 IDE 中的代码到您现有的技能。 只需几次单击，添加版本控制、 更新、 单一登录、 身份代理、 独立的存储，和到您现有的 web 应用程序的性能监视。  访问丰富的库作为构造块用于加快您的开发。 经验与先进功能，如持续集成、 实时网站调试和业界领先的 Visual Studio IDE 的无与伦比的开发人员工作效率。
- **企业级**的 Web 应用程序可用于构建和宿主安全任务关键型应用程序。 构建 Active Directory 集成的业务应用程序安全地连接到内部的资源，然后他们驻留在 ISO、 SOC2，并且符合 PCI 安全的云平台。 所有在享受企业级服务级别协议。
- **全球范围内**的 Web 应用程序进行了优化，在全局数据中心基础架构上提供可用性和自动缩放。 轻松地应用程序向上或向下按需扩展。 提供虚拟机内和跨不同地理区域的高可用性。 复制数据和托管服务在多个位置是快速、 方便、 制作简单，只需单击一次鼠标扩展到新的区域和地理区域。  

## Web 应用程序概念 ##

- **Web 应用程序库**的不断增长的现有 web 应用程序模板的列表中选择。 包如 Wordpress Joomla，Drupal 的一键式安装 OSS 应用社区充分利用。 获得您应用程序的开发过程开始右通过利用像.NET MVC，Django CakePHP 框架。
- **自动缩放**的 Web 应用程序使您能够快速向上扩展或不是要处理任何传入的客户负载。 手动选择的数量和大小的虚拟机或设置自动扩展来扩展服务器基于负载或日程安排。
- **持续集成**-设置持续集成和部署工作流与 VSO、 GitHub、 TeamCity、 编写或 BitBucket--使您能够自动生成、 测试和部署 web 应用程序在每个成功的代码签入或集成测试。
- **部署插槽**-实现[分阶段部署] [插槽]验证您在预生产环境中是相同于 Azure 应用程序投入生产 web 应用程序的代码。 当满足，通过执行交换操作释放零停机时间与您的应用程序的新版本。 
- **在生产环境中的测试**-采取分步部署到下一个级别并执行 A / B 测试，以验证新的代码与您实时通信可配置部分。 
- **Webjobs** -在 Web 应用程序的虚拟机上运行的任何程序或脚本。 运行作业不断地或在日程安排和规模要在多个虚拟机上运行。 使用 Azure [WebJobs SDK][Webjobs] Azure 存储或服务总线相结合。
- **混合连接**— 使用[混合连接](../integration-hybrid-connection-overview.md)和[VNET](../app-service-web/web-sites-integrate-with-vnet.md)访问内部数据。

## 入门教程 ##
若要开始使用 Web 应用程序，请按照[创建 ASP.NET web 应用程序] [创建]教程。

在 Azure 应用程序服务平台上的详细信息，请参阅[Azure 应用程序服务][appservice]。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 旧的门户与新门户的更改的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)

[appservice]: ../app-service/app-service-value-prop-what-is.md
[创建]: web-sites-dotnet-get-started.md
[Webjobs]: websites-dotnet-webjobs-sdk-get-started.md
[插槽]: web-sites-staged-publishing.md

 

测试
