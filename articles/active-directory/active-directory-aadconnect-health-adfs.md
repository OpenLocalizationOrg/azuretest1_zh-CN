---
ms.openlocfilehash: 09b72783f10c71aec418543c08354ff065635775
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="使用 Azure 广告健康与 AD FS |Microsoft Azure" 
    description="这是 Azure AD 连接状况页面如何监视您在内部 AD FS 基础结构。" 
    services="active-directory" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/14/2015" 
    ms.author="billmath"/>

# 与 AD FS 使用 Azure AD 连接运行状况 
以下文档是特定的 AD FS 基础结构使用 Azure AD 连接运行状况监视。

## AD fs 的警报
Azure AD 连接的运行状况警报部分提供活动通知的列表。 每个通知包含相关的信息、 解决方法步骤和相关文档的链接。 通过选择一个活动或已解决的警报中，您将看到的其他信息，以及您可以采取的步骤来解决警报，以及链接到其他文档的新刀片。 您还可以查看历史数据，在过去已解决的警报。

通过选择的通知将为您提供其他信息，以及步骤即可解决的警报和链接到其他文档。

![Azure AD 连接健康门户](./media/active-directory-aadconnect-health/alert2.png)



## AD fs 使用情况分析
Azure 的 AD 连接健康使用率分析分析联合身份验证服务器的身份验证通迅流。 选择使用率分析框中将打开使用率分析刀片式服务器，它将显示您的度量标准和分组。

>[AZURE.NOTE] 为了使用 AD FS 使用使用率分析功能，您必须确保启用 AD FS 的审核。 有关详细信息，请参阅[启用 AD FS 的审核](active-directory-aadconnect-health-operations.md#enable-auditing-for-ad-fs)。 

![Azure AD 连接健康门户](./media/active-directory-aadconnect-health/report1.png)

若要选择其他度量，请指定一个时间范围，或更改分组，只需使用状况分析图表上右键单击，并选择编辑图表。 然后您可以指定时间范围、 更改或选择指标和更改分组。 可以查看基于不同的"标准"身份验证流量的分布和分组使用相关"分组依据"参数，如下所述每个跃点计数。

| 公制 | 分组依据 | 意味着什么分组和为什么它很有用？ |
| ------ | -------- | -------------------------------------------- |
| 总请求︰ 通过联合身份验证服务来处理请求的总数量 | 所有 | 这将显示如果不进行分组的请求总数的计数。 |
|  | 应用程序	 | 此选项将一组基于目标依赖当事方的请求总数。 这种分组可用于了解哪些应用程序正在接收多少百分比的总通信量。 |
|  | 服务器 | 此选项将一组基于处理请求的服务器的请求总数。 这种分组可用于了解总的流量的负载分布。 |
|  | 工作区的连接 | 此选项将组总请求根据请求来自联接的工作场所的设备 （已知）。 这种分组可用于了解是否使用未知的身份基础结构的设备访问您的资源。 |
|  | 身份验证方法 | 此选项将一组基于用于身份验证的身份验证方法的请求总数。 这种分组可用于了解公共获取用于身份验证的身份验证方法。 以下是可能的身份验证方法 <ol> <li>Windows 集成身份验证 (Windows)</li> <li>基于表单的身份验证 （窗体）</li> <li>SSO （单一登录）</li> <li>X509 证书身份验证 （证书）</li> <br>请注意，请求被视为 SSO （单一登录） 是否联合身份验证服务器收到请求 SSO 的 cookie。 在这种情况下，如果该 cookie 是有效的用户不要求提供凭据并获取对应用程序的无缝访问。 这是常见的如果您有多个联合服务器所受的信赖方。 |
|  | 网络位置 | 此选项将一组基于网络位置的用户的请求总数。 它可以是任何一个企业内部网或外部网。 这种分组很有用知道百分之多少的流量来自企业内部网与外部网。 |
| 失败的请求总数︰ 失败联合身份验证服务处理的请求总数。 <br> （该指标只具有 Windows Server 2012 R2 的 AD FS）| 错误类型 | 这将显示基于预定义的错误类型的错误数。 这种分组可用于理解错误的常见类型。 <ul><li>不正确的用户名或密码︰ 由于错误的用户名或密码错误。</li> <li>"外部网锁定": 从外部网已被锁定的用户接收到的请求引起的故障 </li><li> "过期密码": 由于故障而导致用户登录已过期的密码。</li><li>"禁用帐户": 由于用户使用已禁用的帐户登录失败。</li><li>"身份验证设备": 由于故障而导致无法使用设备的身份验证进行身份验证的用户。</li><li>"用户证书身份验证": 用户无法验证证书无效原因引起的故障。</li><li>"MFA": 由于用户不使用多因素身份验证进行身份验证失败。</li><li>"其他凭据":"颁发授权": 授权失败引起的故障。</li><li>"颁发委派": 颁发委派错误引起的故障。</li><li>"标记验收": 由于 ADFS 拒绝第三中的令牌失败方身份标识提供程序。</li><li>"协议": 由于协议错误而失败。</li><li>"未知": 全部捕获。 任何其他失败不符合定义的类别。</li> |
|  | 服务器 | 这将一组基于服务器的错误。 这将有助于理解跨服务器的错误分布。 分配不均，可能是服务器处于故障状态的指示器。 |
|  | 网络位置 | 这将一组基于网络位置 （内部网与外部网） 的请求的错误。 这将有助于了解失败的请求的类型。 |
|  | 应用程序	 | 这将一组基于 （依赖方） 的目标应用程序的故障。 这将有助于了解哪个目标应用程序所看到的错误最多。 |
| 用户计数︰ 平均的系统中的活动的唯一用户数 | 所有 | 这样，用户在选定的时间段中使用联合身份验证服务的平均数目。 未分组的用户。 <br>平均值将取决于所选的时间片。 |
|  | 应用程序	 | 这将一组基于 （依赖方） 的目标应用程序的用户的平均数量。 这将有助于了解多少用户正在使用的应用程序。 |


## AD fs 监视性能
Azure AD 连接健康性能监控提供了监视信息的度量标准。 通过选择监视框中，刀片式服务器将打开标准提供详细的信息。


![Azure AD 连接健康门户](./media/active-directory-aadconnect-health/perf1.png)


通过选择刀片式服务器顶部的筛选器选项，您可以通过筛选服务器以查看单个服务器的规格。 若要更改度量标准，只是下监视刀片式服务器监视图表上右键单击，并选择编辑图表。 然后，从开辟新刀片，可以从下拉列表中选择其他度量并指定查看性能数据的时间范围。




## 相关的链接

* [Azure AD 连接的运行状况](active-directory-aadconnect-health.md)
* [Azure AD 连接 AD fs 健康代理安装](active-directory-aadconnect-health-agent-install-adfs.md)
* [Azure AD 连接健康操作](active-directory-aadconnect-health-operations.md)
* [Azure AD 连接健康常见问题解答](active-directory-aadconnect-health-faq.md)

测试
