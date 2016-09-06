---
ms.openlocfilehash: 688f73497c98b630c85b2788b87d84903119d381
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何在我的组织内发现未批准的云应用程序使用" 
    description="本主题描述了什么是云应用程序 Discvery 是，为什么要用它。" 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="swadhwa" 
    editor="lisatoft"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/23/2015" 
    ms.author="markusvi"/>

# 如何在我的组织内发现未批准的云应用程序使用

在现代企业中，IT 部门人员通常不了解所有的云应用程序由用户用来做他们的工作。
因此，管理员通常需要考虑问题结合到企业数据、 可能的数据泄漏和其他应用程序中固有的安全风险进行未经授权的访问。
因为他们不知道多少或哪些应用程序使用了，甚至开始工作以处理这些风险的计划似乎令人望而生畏的建筑物。

您可以通过使用云应用程序发现解决这些问题。
云应用程序发现是 Azure 活动目录使您能够发现是由您的组织中的员工使用的云应用程序的高级功能。 


**云应用程序查询，您可以︰**

* 发现应用程序中使用和度量值使用的用户数量、 卷的流量或的 web 请求到应用程序。 
* 标识要使用的应用程序的用户。 
* 为导出数据添加离线分析。 
* 优先考虑使 IT 控制和集成应用程序方便地启用单一登录和用户管理的应用程序。 

云应用程序查询，数据检索部分通过在您的环境中的计算机运行的代理程序。
代理程序捕获应用程序使用信息是转移到云应用程序发现服务的安全、 加密通道发送。
云应用程序发现服务评估数据并生成的报告，您可以使用它来分析您的环境。


<center>![云应用程序发现的工作原理](./media/active-directory-cloudappdiscovery/cad01.png)</center>

##下一步行动


* 若要了解有关如何为云应用程序发现工作原理的详细信息，请参阅[获取启动与云应用程序发现](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx) 
* 安全和隐私注意事项，请参见[云应用程序发现安全和隐私注意事项](active-directory-cloudappdiscovery-security-and-privacy-considerations.md) 
* 有关部署云应用程序探测代理的企业环境中的信息︰ 
 * 活动目录组策略管理[云应用程序发现组策略部署指南](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)，请参阅 
 * Microsoft 系统中心配置管理器中，看到了[云应用程序发现系统中心部署指南](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx) 
 * 包含自定义端口的代理服务器，请参阅[云应用程序发现注册表设置为使用自定义端口的代理服务器](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md) 





**其他资源**


* [云应用程序发现-代理更改日志 ](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [云应用程序发现-常见问题](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)



测试
