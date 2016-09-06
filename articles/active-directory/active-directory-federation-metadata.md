---
ms.openlocfilehash: 0f88e6fac70eab57773e7adf6524534d898ef8fe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="联合元数据"
   description="描述的元数据终结点和该服务的元数据文档中的内容接受必须使用 Azure AD 标记。"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="05/30/2015"
   ms.author="mbaldwin"/>

# 联合元数据
Azure 活动目录 (AD Azure) 发布的服务被配置为接受 Azure Active Directory 颁发的安全令牌的联合元数据文档。 [Web 服务联合身份验证语言 （WS 联合身份验证） 1.2 版](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html)，扩展[为 OASIS 安全声明标记语言 (SAML) 2.0 版的元数据](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf)描述了联合元数据文档格式。

本主题列出的元数据终结点，并说明接受 Azure AD 标记的服务需要使用元数据文档中的内容。

##特定于租户的并独立于租户的元数据终结点

Azure 广告发布特定于租户的并独立于租户的终结点。 特定于租户的端点用于特定租户。 特定于租户的联合元数据包含有关租户，包括特定于租户的颁发者和终结点信息的信息。 应用程序将访问限制为单个租户使用特定于租户的终结点。

独立于租户的终结点提供所有 Azure AD 承租人对通用的信息。 此信息适用于位于*login.windows.net*的承租人，并在租户之间共享。 由于它们不与任何特定租户对于多租户应用程序，建议独立于租户的终结点。

## 联合身份验证的元数据终结点

Azure 广告发布联合元数据在*https://login.windows.net/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml*，其中的值<TenantDomainName>可以是"普通"或特定于租户的值。
终结点是可寻址的的所以可以转到地址的站点查看承租人联合元数据。

**特定于租户的终结点**，以<TenantDomainName>可以是以下类型之一︰

- 已注册的域名 Azure 的广告的租户，如︰ contoso.onmicrosoft.com。

- 不可变租户 id 域，例如，72f988bf-86f1-41af-91ab-2d7cd011db45。

**独立于租户的终结点**，以<TenantDomainName>是很**常见**。 该名称指示，只有联合元数据元素所共有的所有驻留在 login.windows.net 的 Azure AD 承租人。

例如，一个特定于租户的终结点可能是*https://login.windows.net/contoso.onmicrosoft.comFederationMetadata/2007-06/FederationMetadata.xml*。 独立于租户的终结点是*https://login.windows.net/common/FederationMetadata/2007-06/FederationMetadata.xml*。

## 联合身份验证元数据的内容

以下部分提供了使用 Azure 广告发出的标记的服务所需的信息。

### EntityID

**EntityDescriptor**元素中包含的**EntityID**属性。 **EntityID**属性的值表示的颁发者，即安全令牌服务 (STS) 颁发令牌。 很重要的以便您可以确认的租户发放给一个令牌验证颁发者。

下面的元数据显示与**EntityID**元素的示例特定于租户的**EntityDescriptor**元素。

    <EntityDescriptor 
    xmlns="urn:oasis:names:tc:SAML:2.0:metadata" 
    ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b" 
    entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">

独立于租户的终结点中的**EntityID**提供一个模板，可用于生成特定于租户的**EntityID**值。 独立于租户的终结点中的"{0} 租户}"文本替换**TenantID**声明从令牌值。 所得到的值将令牌颁发者相同。 该策略允许的多租户应用程序以验证给定的租户的颁发者。

下面的元数据显示了一个示例独立于租户的**EntityID**元素。 在此元素中，{租户} 是文本，不是一个占位符。

    <EntityDescriptor 
    xmlns="urn:oasis:names:tc:SAML:2.0:metadata" 
    ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd" 
    entityID="https://sts.windows.net/{tenant}/">

### 令牌签名证书
当服务收到 Azure AD 租户颁发的令牌时，令牌的签名必须用发布联合元数据文档中的签名密钥进行验证。  

联合元数据包括承租人使用令牌签名证书的公钥部分。 **KeyDescriptor**元素中显示的证书的原始字节。 令牌签名证书是有效的签名时才**使用**属性的值**签名**。

通过 Azure 广告发布联合元数据文档可以有多个签名密钥，如 Azure 广告正在准备更新签名证书。 当联合元数据文档包含多个证书时，验证标记的服务应在文档中支持的所有证书。

下面的元数据签名的密钥显示示例**KeyDescriptor**元素。

    <KeyDescriptor use="signing">
    <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
    <X509Data>
    <X509Certificate>
    MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
    </X509Certificate>
    </X509Data>
    </KeyInfo>
    </KeyDescriptor>

**KeyDescriptor**元素出现在两个位置中联合元数据文档;在 WS 联合身份验证的特定部分和 SAML 特定部分。 在这两个部分中发布证书将是相同的。

在 WS 联合身份验证的特定部分中，WS 联合身份验证元数据读取器将读取证书从**RoleDescriptor**元素与**SecurityTokenServiceType**类型。

下面的元数据显示了一个示例**RoleDescriptor**元素。

    <RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706"
    xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">`

在特定 SAML 的部分中，WS 联合身份验证元数据读取器会从**IDPSSODescriptor**元素中读取证书。

下面的元数据显示了一个示例**IDPSSODescriptor**元素。

    <IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">

没有任何特定于租户的并独立于租户的证书格式差别。

### WS 联合身份验证端点 URL
联合元数据包含的 URL 的 Azure 广告使用单一登录和注销 WS 联合身份验证协议中的一个。 此终结点出现在**PassiveRequestorEndpoint**元素中。

下面的元数据显示为特定于租户的终结点的示例**PassiveRequestorEndpoint**元素。

    <fed:PassiveRequestorEndpoint>
    <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
    <Address>
    https://login.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
    </Address>
    </EndpointReference>
    </fed:PassiveRequestorEndpoint>

对于独立于租户的端点，WS 联合身份验证 URL 出现 WS 联合身份验证终结点，如下面的示例中所示。

    <fed:PassiveRequestorEndpoint>
    <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
    <Address>
    https://login.windows.net/common/wsfed
    </Address>
    </EndpointReference>
    </fed:PassiveRequestorEndpoint>

### SAML 协议端点 URL

联合元数据包括 Azure AD 使用单一登录和注销 SAML 2.0 协议中单的 URL。 这些终结点将出现在**IDPSSODescriptor**元素中。

登录和注销 Url 显示**SingleSignOnService**和**SingleLogoutService**元素中。
下面的元数据显示示例**PassiveResistorEndpoint**为特定于租户的终结点。

    <IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
    …
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.windows.net/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https:// login.windows.net/contoso.onmicrosoft.com /saml2" />
    </IDPSSODescriptor>

同样为常见的 SAML 2.0 协议终结点的终结点都发布在独立于租户的联合元数据，如下面的示例中所示。 

    <IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
    …
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.windows.net/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.windows.net/common/saml2" />
    </IDPSSODescriptor>

## 请参见
[Azure 的活动目录的身份验证协议](active-directory-authentication-protocols.md)

[Azure 活动目录开发人员指南](active-directory-developers-guide.md)测试
