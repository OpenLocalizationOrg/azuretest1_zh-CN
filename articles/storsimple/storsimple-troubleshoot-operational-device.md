---
ms.openlocfilehash: 6c6df2f94cfcdb025c106f659858d93c0eadf0c2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="疑难解答已部署的 StorSimple 设备 |Microsoft Azure"
   description="描述如何诊断和解决目前已部署和运营的 StorSimple 设备上发生的错误。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/31/2015"
   ms.author="v-sharos" />

# 运营的 StorSimple 设备的疑难解答

## 概述

这篇文章提供了有用的解决配置问题的故障排除指南遇到 StorSimple 设备后部署和操作。 它描述了常见的问题，可能的原因，并建议采取的步骤来帮助您解决在运行 Microsoft Azure StorSimple 时可能遇到的问题。 此信息适用于 StorSimple 内部物理设备和 StorSimple 的虚拟设备。

在这篇文章，您可以找到在 Microsoft Azure StorSimple 操作过程中可能遇到的错误代码的列表的末尾，步骤以及需要解决这些错误。 

## 安装向导过程的操作设备

您可以使用安装向导 ([调用 HcsSetupWizard][1]) 检查该设备的配置，并采取纠正措施，如有必要。

以前的配置和操作的设备上运行安装向导时，流程将是不同的。 您可以更改下面的项︰

- IP 地址、 子网掩码和网关
- 主 DNS 服务器
- 主 NTP 服务器
- 可选的 web 代理服务器配置

安装向导不执行相关的密码集合和设备注册的操作。

## 在安装向导的后续运行期间发生的错误

下表介绍了当您运行安装向导在操作的设备上，原因可能导致此错误，可能会遇到，建议措施来解决这些错误。 

| No. | 错误消息或条件 | 可能的原因 | 建议的操作 |
|:--- |:-------------------------- |:--------------- |:------------------ |
|  1  | 错误 350032︰ 该设备已被禁用。 | 如果您停用的设备上运行安装向导，您将看到此错误。 | 下一步行动[与 Microsoft 客户支持联系](storsimple-contact-microsoft-support.md)。 已停用的设备不能放在服务。 可以重新激活该设备之前，可能需要出厂重置。 |
|  2  | 调用-HcsSetupWizard: ERROR_INVALID_FUNCTION (从 HRESULT 异常︰ 0x80070001) | DNS 服务器更新失败。 DNS 设置都是全局设置，并且应用在所有已启用的网络接口。 | 启用该接口并再次应用了 DNS 设置。 因为这些设置是全局的这可能会破坏网络中的其他已启用的接口。 |
|  3  | 该设备显示为联机在 StorSimple 管理器服务门户中，但当您尝试完成的最低设置，并保存配置，则操作将失败。 | 在初始安装时，web 代理未配置，即使没有实际的代理服务器到位。 | 使用[测试 HcsmConnection cmdlet][2]查找错误。 [与 Microsoft 客户支持](storsimple-contact-microsoft-support.md)如果您无法解决此问题。 |
|  4  | 调用 HcsSetupWizard︰ 值不在预期范围内。 | 一个不正确的子网掩码将产生此错误。 可能的原因是︰ <ul><li> 子网掩码是丢失或为空。</li><li>Ipv6 前缀的格式不正确。</li><li>接口是云计算带来的但是网关丢失或不正确。</li></ul>请注意，数据 0 云-是否自动启用通过安装向导配置。 | 要确定此问题，请使用子网 0.0.0.0 或 256.256.256.256，，然后查看输出。 根据需要请输入正确的值的子网掩码、 网关和 Ipv6 前缀。 |
 
## 错误代码

按数字顺序列出了错误。

|错误号|错误文本或说明|建议的用户操作|
|:---|:---|:---|
|10502|访问您的存储帐户时遇到错误。|等待几分钟然后再试一次。 如果此错误仍然存在，请联系 Microsoft 技术支持为下一步行动。|
|40017|无法解析中备份集的磁盘。|如果此错误仍然存在，请联系 Microsoft 技术支持为下一步行动。|
|40018|不能解决任何在备份集的磁盘中。|如果此错误仍然存在，请联系 Microsoft 技术支持为下一步行动。|
|390061|系统正忙或不可用。|等待几分钟然后再试一次。 如果此错误仍然存在，请联系 Microsoft 技术支持为下一步行动。|
|390143|错误，错误代码为 390143。 （未知的错误）。|如果错误仍然存在，请联系 Microsoft 技术支持下一步行动。|

## 下一步行动

如果您无法解决此问题，请[与 Microsoft 支持部门联系](storsimple-contact-microsoft-support.md)以获得帮助。 


[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
