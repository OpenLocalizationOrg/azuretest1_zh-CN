---
ms.openlocfilehash: 8ababb31d5af1a348d7d7c581526de979e6c0d6b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用自定义 Azure AD 应用程序代理中的域"
    description="介绍如何使用 Azure AD 应用程序代理中自定义的域。"
    services="active-directory"
    documentationCenter=""
    authors="rkarlin"
    manager="msStevenPo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/06/2015"
    ms.author="rkarlin"/>

# 使用自定义的域的 Azure AD 应用程序代理
> [AZURE.NOTE] 应用程序代理服务器是仅当您升级到特优或 Azure Active directory 的基本版才可用的功能。 有关详细信息，请参见[Azure Active Directory 版本](https://msdn.microsoft.com/library/azure/dn532272.aspx)。

使用默认域使您可以将相同的 URL 设置为，以便您的用户只需一个 URL 来访问该应用程序，无论，其中他们所访问的它使您能够创建应用程序访问权限面板中的某个快捷方式，请务必访问该应用程序的内部和外部 URL。 如果您使用 Azure AD 应用程序代理服务器提供的默认域，没有任何附加的配置需要启用您的域。 中，您使用自定义的域时，有几件事情需要做，以确保应用程序代理服务器能够识别您的域，并验证其证书。

## 选择您自定义的域

1. 发布您的应用程序，根据与应用程序代理的发布应用程序中的说明进行操作。
2. 应用程序出现在应用程序列表后，请选择它并单击**配置**。
3. 在**外部 URL**，请输入您自定义的域。
4. 如果您的外部 URL 是 https，将提示您要上载证书，所以该 Azure 可以验证应用程序的 URL。 您还可以将与该应用程序的外部 URL 匹配的通配符证书。 此域必须是您的[Azure 验证域](https://msdn.microsoft.com/library/azure/jj151788.aspx)列表中。 Azure 必须有证书的应用程序或通配符证书相匹配的应用程序的外部 URL 的域 URL。
5. 请确保要添加的 DNS 记录，将该内部 URL 路由到的应用程序使您可以拥有相同的 URL，以及内部和外部访问某个快捷方式为应用程序用户的应用程序列表中。

## 有关使用自定义的域的常见问题

问︰ 可以选择无需再次上载它已上载证书？ <br>
答︰ 以前上载的证书会自动绑定到应用程序，并且没有一个证书匹配应用程序的主机名。 <br>
…<br>
问︰ 如何添加证书和哪种格式应导出的证书要上载中？ <br>
答︰ 证书应该再上载从应用程序配置页。 该证书应 PFX 文件。 <br>
…<br>
问︰ 可以使用 ECC 证书吗？ <br>
答︰ 签名方法上的没有明确限制。 <br>
…<br>
问︰ 可以使用 SAN 证书吗？ <br>
答︰ 是的。<br>
…<br>
问︰ 可以使用通配符证书吗？ <br>
答︰ 是的。 <br>
…<br>
问︰ 可以在每个应用程序使用不同的证书吗？ <br>
答︰ 是的除非两个应用程序共享同一个外部主机。 <br>
…..<br>
问︰ 如果我在注册一个新域，可以使用此域？ <br>
答︰ 是的纸的域的列表是从为租户已验证的域列表。 <br>
…<br>
问︰ 如果证书过期怎么办？ <br>
答︰ 您将在应用程序配置页中的证书部分得到警告。 当用户尝试访问该应用程序时，将弹出一条安全警告。 <br>
…<br>
问︰ 如果我想要替换为给定的应用程序证书怎么办？ <br>
答︰ 上载一个新证书从应用程序配置页<br>
…<br>
问︰ 可以删除证书并将其替换？ <br>
答︰ 当您上载一个新证书，如果旧的证书未被另一个应用程序使用，则会自动删除<br>
…<br>
问︰ 什么时发生的证书已被吊销<br>
答︰ 吊销检查不会对证书执行。 当用户试图访问该应用程序，这取决于浏览器中，可能会出现安全警告。<br>
…<br>
问︰ 可以使用自签名的证书？ <br>
答︰ 是的允许使用自签名的证书。 请注意，是否您正在使用的私有证书颁发机构，证书的 CDP （证书吊销点分发点） 应公用。 <br>
...<br>
问︰ 有一个位置，以便查看我组织的所有证书吗？ <br>
答︰ 这不是支持当前版本。<br>



## 其他资源

* [作为一个组织注册 azure](..sign-up-organization.md)
* [Azure 的标识](..fundamentals-identity.md)

测试
