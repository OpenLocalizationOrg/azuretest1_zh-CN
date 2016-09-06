---
ms.openlocfilehash: 7bdcc118f46facfc472f52c2ff627afc5d843dbc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="服务总线中继邮件"
    description="服务总线中继的概述。"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/04/2015"
    ms.author="sethm"/>


# 服务总线中继邮件

服务总线的核心组件是一种集中 （但高负载平衡） 中继服务，使您可以构建混合在 Azure 数据中心和自己内部的企业环境中运行的应用程序。  中继服务支持各种不同的传输协议和 web 服务标准。 这包括 SOAP，WS-*，甚至其余部分。 服务总线中继有助于通过使您能够安全地公开与公有云，为公司的企业网络内驻留而无需打开防火墙的连接，或者需要侵入性对企业网络基础结构的 Windows 通讯基础 (WCF) 服务混合应用程序中。 

![中继的概念](./media/service-bus-relay-overview/sb-relay-01.png)

中继服务支持传统单向消息，请求/响应消息，以及对等消息。 它还支持在互联网范围内启用发布/订阅方案和增加了点到点效率的双向套接字通信事件分发。 

在中继消息传递模式中，内部服务连接到出站端口通过中继服务，并创建双向套接字绑定到一个特定集合地址的通信。 客户机然后可以将邮件发送到中继服务目标集合地址与内部服务通信。 中继服务将再""将邮件中继到内部服务通过双向的插座已经到位。 客户端不需要直接连接到内部服务，不需要知道位置服务所驻留，和内部服务不需要任何入站的端口在防火墙上打开。

后端服务和使用 WCF"中继"绑定一套中继服务之间启动连接。 在幕后，中继绑定映射到用于创建与云中的服务总线集成的 WCF 通道组件的新传输绑定元素。 

## 下一步行动

有关服务总线中继的详细信息，请参阅下列主题。

- [Azure 服务总线体系结构概述](fundamentals-service-bus-hybrid-solutions.md)

- [如何使用服务总线中继服务](service-bus-dotnet-how-to-use-relay.md)

 