---
ms.openlocfilehash: 54a27e8876da3cc6ec954e95d23311763ed3dd6a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="关于 VPN 站点到站点 Azure 虚拟网络连接的设备 |Microsoft Azure"
   description="了解有关 VPN 设备和 Azure 虚拟网络站点到站点的 VPN 网关连接的 IPsec 参数。"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carolz"
   editor="tysonn" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/07/2015"
   ms.author="cherylmc" />

# 关于 VPN 站点到站点虚拟网络连接的设备

VPN 设备需要配置站点到站点 VPN 连接。 站点到站点的连接可用于创建混合云解决方案，或无论您何时需要安全连接内部网络和虚拟网络之间。 本文讨论了兼容的 VPN 设备和配置参数。 

务必要了解，除了兼容的 VPN 设备，站点对站点连接也需要面向公众的 IPv4 地址。 此外，您需要选择将您的解决方案提供支持的网关类型。 不是所有 VPN 设备都支持的网关的所有类型。 请参阅[VPN 网关](vpn-gateway-about-vpngateways.md)验证网关，您需要创建您的解决方案的类型。                                                                                                                                                                                



## VPN 设备

我们验证一套标准站点到站点 (S2S) 的 VPN 设备与设备供应商的合作关系。 已知与虚拟网络兼容的 VPN 设备的列表，请参阅下面的明[兼容的 VPN 设备](#compatible-vpn-devices)。 在此列表中包含的设备系列中的所有设备应都使用 Azure VPN 网关。 要配置 VPN 设备的帮助，请参阅对应于相应设备系列设备配置示例。

除非另有说明，高性能 VPN 网关和动态路由的 VPN 网关的规格都是相同的。 例如，验证的 Azure 动态路由选择 VPN 网关与兼容的 VPN 设备也将与新 Azure 高性能 VPN 网关。


### 兼容的 VPN 设备

我们已与 VPN 设备供应商合作，以共同确认特定 VPN 设备系列。 下面的部分列出了所有已知的工作与我们的 VPN 网关的设备系列。 列出的设备家族的成员的所有设备都称为工作除非提到了例外。 如果您的设备不在列表中，请参阅[兼容列表上没有的设备](#devices-not-on-the-compatible-list)。



对于 VPN 设备支持，请与设备制造商联系。



| **供应商**                      | **设备系列**                                        | **最小操作系统版本**                             | **静态路由**                                                                                                                                                                                                             | **动态路由**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 联合的 Telesis                  | AR 系列 VPN 路由器                                    | 2.9.2                                              | 即将推出                                                                                                                                                                                                                                          | 不兼容                                                                                                                                                                                               |
| Barracuda 公司网络        | Barracuda NG 防火墙                 | Barracuda NG 防火墙 5.4.3  | [Barracuda NG 防火墙](https://techlib.barracuda.com/display/BNGV54/How%20to%20Configure%20an%20IPsec%20Site-to-Site%20VPN%20to%20a%20Windows%20Azure%20VPN%20Gateway)| 不兼容                                                                                                                                                                                               |
| Barracuda 公司网络        |  Barracuda 防火墙                 | Barracuda 防火墙 6.5 | [Barracuda 防火墙](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | 不兼容                                                                                                                                                                                               |
| 织锦式                         | Vyatta 5400 vRouter                                      | 虚拟路由器 6.6R3 GA                            | [配置说明](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | 不兼容                                                                                                                                                                                               |
| 检查点                     | 安全网关                                         | R75.40 R75.40VS                                     | [配置说明](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [配置说明](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Cisco ASA 示例](https://msdn.microsoft.com/library/azure/dn133793.aspx)                                                                                                                                                                        | 不兼容                                                                                                                                                                                               |
| Cisco                           | ASR                                                      | IOS 15.1 （静态） IOS 15.2 （动态）                | [Cisco ASR 示例](https://msdn.microsoft.com/library/azure/dn133802.aspx)                                                                                                                                                                        | [Cisco ASR 示例](https://msdn.microsoft.com/library/azure/dn133802.aspx)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 于 15.0 （静态） IOS 15.1 （动态）               | [Cisco ISR 示例](https://msdn.microsoft.com/library/azure/dn133800.aspx)                                                                                                                                                                        | [Cisco ISR 示例](https://msdn.microsoft.com/library/azure/dn133800.aspx)                                                                                                                                |
| Citrix 服务器                          | CloudBridge 将丢失装置或 VPX 虚拟应用装置       | N/A                                                | [集成说明](https://www.citrix.com/welcome.html?resource=%2Fdownloads%2Fcloudbridge%2Fbetas-and-tech-previews%2Fcloudbridge-azure-integration)                                                                                                                                                                            | 不兼容                                                                                                                                                                                               |
| 戴尔使 SonicWALL                  | TZ 系列，NSA 系列、 SuperMassive 系列、 E 类 NSA 系列 | SonicOS 5.8.x，SonicOS 5.9.x，SonicOS 6.x          | [配置说明](https://www.sonicwall.com/app/projects/file_downloader/document_lib.php?t=TN&id=348)                                                                                                                                    | 不兼容                                                                                                                                                                                               |
| F5                              | BIG-IP 系列                                            | N/A                                                | [配置说明](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | 不兼容                                                                                                                                                                                               |
| Fortinet                        | FortiGate                                                | 5.0.7 FortiOS                                      | [配置说明](http://docs.fortinet.com/fortigate/admin-guides)                                                                                                                                                                          | [配置说明](http://docs.fortinet.com/fortigate/admin-guides)                                                                                                                                  |
| 互联网计划日本 (IIJ) | SEIL 系列                                              | SEIL / X 4.60，SEIL/B1 4.60 SEIL/x86 3.20            | [配置说明](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | 不兼容                                                                                                                                                                                               |
| 刺柏                         | SRX                                                      | JunOS 10.2 （静态） JunOS 11.4 （动态）            | [刺柏 SRX 示例](https://msdn.microsoft.com/library/azure/dn133794.aspx)                                                                                                                                                                      | [刺柏 SRX 示例](https://msdn.microsoft.com/library/azure/dn133794.aspx)                                                                                                                              |
| 刺柏                         | J 系列                                                 | JunOS 10.4r9 （静态） JunOS 11.4 （动态）          | [刺柏 J 系列示例](https://msdn.microsoft.com/library/azure/dn133799.aspx)                                                                                                                                                                 | [刺柏 J 系列示例](https://msdn.microsoft.com/library/azure/dn133799.aspx)                                                                                                                         |
| 刺柏                         | ISG                                                      | ScreenOS 6.3 （静态和动态）                  | [刺柏 ISG 示例](https://msdn.microsoft.com/library/azure/dn133797.aspx)                                                                                                                                                                      | [刺柏 ISG 示例](https://msdn.microsoft.com/library/azure/dn133797.aspx)                                                                                                                              |
| 刺柏                         | SSG                                                      | ScreenOS 6.2 （静态和动态）                  | [刺柏 SSG 示例](https://msdn.microsoft.com/library/azure/dn133796.aspx)                                                                                                                                                                      | [刺柏 SSG 示例](https://msdn.microsoft.com/library/azure/dn133796.aspx)                                                                                                                              |
| Microsoft                       | 路由和远程访问服务                        | Windows Server 2012                                | 不兼容                                                                                                                                                                                                                                       | [路由和远程访问服务 (RRAS) 的示例](https://msdn.microsoft.com/library/azure/dn133801.aspx)                                                                                           |
| Openswan                        | Openswan                                                 | 2.6.32                                             | （即将提供）                                                                                                                                                                                                                                        | 不兼容                                                                                                                                                                                               |
| 帕洛阿图网络              | 所有设备运行平移-OS 5.0 或更高版本                | PAN OS 5 x 或更高版本                               | [帕洛阿图网络](https://support.paloaltonetworks.com/)                                                                                                                                                                                          | 不兼容                                                                                                                                                                                               |
| Watchguard                      | 所有                                                      | Fireware XTM v11.x                                 | [配置说明](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | 不兼容                                                                                                                                                                                               |


### 设备不兼容列表


如果不显示您已知兼容的 VPN 设备列表中的设备，并且想要为您的 VPN 连接中使用该设备，您需要验证它符合[网关要求](vpn-gateway-about-vpngateways.md#gateway-requirements)表中所述的最低要求。 设备符合的最低要求也应适用于虚拟网络。 请有关其他支持和配置说明，与设备制造商联系。


## 编辑设备配置示例

下载提供的 VPN 设备配置示例后，您将需要替换的某些值，以反映您的环境的设置。 

**若要编辑此示例︰**

1. 打开示例使用记事本。 
1. 搜索并替换所有 <*文字*> 字符串使用适用于您的环境的值。 请务必包括 < 和 >。 指定一个名称时，您选择的名称应该是唯一的。 如果命令不起作用，请参阅设备制造商文档。

| **示例文本**                | **更改为**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | 此对象为您选择的名字。 示例︰ myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | 此对象为您选择的名字。 示例︰ myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | 此对象为您选择的名字。 示例︰ myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | 此对象为您选择的名字。 示例︰ myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | 此对象为您选择的名字。 示例︰ myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | 指定范围。 示例︰ 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | 指定的子网掩码。 示例︰ 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | 指定在部署范围。 示例︰ 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | 指定内部子网掩码。 示例︰ 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | 此特定于虚拟网络的信息，且位于管理门户作为**网关的 IP 地址**。 |
| &lt;SP_PresharedKey&gt;                | 此信息是特定于您的虚拟网络，并位于作为管理密钥管理门户中。          |



## IPsec 的参数

### IKE 阶段 1 安装程序

| **属性**                                       | **静态路由的 VPN 网关** | **动态路由选择 VPN 网关和标准或高性能 VPN 网关** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| IKE 版本                                        | IKEv1                          | IKEv2                                                            |
| Diffie-hellman 组                               | 组 2 （1024 位）             | 组 2 （1024 位）                                               |
| 身份验证方法                              | 预共享的密钥                 | 预共享的密钥                                                   |
| 加密算法                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| 哈希算法                                  | SHA1(SHA128)                   | SHA1(SHA128)                                                     |
| 阶段 1 的安全关联 (SA) 有效期限 （时间） | 28800 秒                 | 28800 秒                                                   |


### IKE 阶段 2 安装程序

| **属性**                                                             | **静态路由的 VPN 网关**                 | **动态路由选择 VPN 网关和标准或高性能 VPN 网关**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| IKE 版本                                                              | IKEv1                                          | IKEv2                                                              |
| 哈希算法                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| 第二阶段安全关联 (SA) 有效期限 （时间）                        | 3600 秒                                  | -                                                                  |
| 第二阶段安全关联 (SA) 生存期 （吞吐量）                  | KB 102,400,000                                 | -                                                                  |
| IPsec SA 的加密和身份验证只提供 （按优先级） | 1.ESP AES256 2。 ESP AES128 3。 ESP 3DES 4。 N/A | 请参阅动态路由网关 IPsec 安全关联 (SA) 提供 |
| 完全向前保密 (PFS)                                            | 否                                             | 是 (DH Group1)                                                    |
| 死等检测                                                      | 不受支持                                  | 支持                                                          |

### 动态路由网关 IPsec 安全关联 (SA) 提供

下表列出了 IPsec SA 的加密和身份验证只提供。 列出提供此项优惠活动是提供或接受的首选顺序。

| **IPsec SA 的加密和身份验证服务** | **作为启动器的 azure 网关**                               | 作为响应方的 azure 网关                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | AH 与 ESP AES_128 与空 HMAC 的 SHA1                      |
| 5                                                 | AH 与 ESP AES_256 与空 HMAC 的 SHA1                      | 与空 HMAC 的 AH SHA1 使用 ESP 3_DES                        |
| 6                                                 | AH 与 ESP AES_128 与空 HMAC 的 SHA1                      | AH MD5 使用 ESP 3_DES 与空 HMAC，建议没有生存期 |
| 7                                                 | 与空 HMAC 的 AH SHA1 使用 ESP 3_DES                        | AH 与 ESP 3_DES SHA1，没有生存期的 SHA1                    |
| 8                                                 | AH MD5 使用 ESP 3_DES 与空 HMAC，建议没有生存期 | AH 与 ESP 3_DES MD5，没有生存期的 MD5                     |
| 9                                                 | AH 与 ESP 3_DES SHA1，没有生存期的 SHA1                    | ESP DES MD5                                                  |
| 10                                                | AH 与 ESP 3_DES MD5，没有生存期的 MD5                     | ESP DES SHA1，没有生存时间                                   |
| 11                                                | ESP DES MD5                                                  | AH 与 ESP DES 空 HMAC SHA1 没有寿命建议        |
| 12                                                | ESP DES SHA1，没有生存时间                                   | AH 与 ESP DES 空 HMAC 的 MD5 没有寿命建议        |
| 13                                                | AH 与 ESP DES 空 HMAC SHA1 没有寿命建议        | AH 与 ESP DES SHA1，没有生存期的 SHA1                      |
| 14                                                | AH 与 ESP DES 空 HMAC 的 MD5 没有寿命建议        | AH 与 ESP DES MD5，没有生存期的 MD5                       |
| 15                                                | AH 与 ESP DES SHA1，没有生存期的 SHA1                      | ESP SHA，没有生存时间                                        |
| 16                                                | AH 与 ESP DES MD5，没有生存期的 MD5                       | ESP MD5，没有生存时间                                        |
| 17                                                | -                                                            | 噢 SHA，没有生存时间                                         |
| 18                                                | -                                                            | AH MD5，没有生存时间                                         |




- 您可以使用动态路由和高性能 VPN 网关，供 Azure 网络内的 VNet 到 VNet 连接指定 IPsec ESP NULL 加密。 

- 跨内部通过互联网的连接，请使用上面的表中列出，以确保关键的通信的安全哈希算法加密与默认 Azure VPN 网关设置。

## 下一步行动


若要了解有关 VPN 网关的详细信息，请参阅[关于 VPN 网关](vpn-gateway-about-vpngateways.md)。

若要配置站点到站点 VPN，请参阅[创建站点到站点 VPN 连接使用的虚拟网络](vpn-gateway-site-to-site-create.md)。



