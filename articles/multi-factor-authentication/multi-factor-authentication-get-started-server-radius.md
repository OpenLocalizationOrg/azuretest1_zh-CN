---
ms.openlocfilehash: 10c1f9117b864ca3b960601107c927aa84e20c31
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="RADIUS 身份验证和 Azure 的多因素身份验证服务器" 
    description="这是部署 RADIUS 身份验证和 Azure 多因素身份验证服务器将协助 Azure 的多因素身份验证页。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>



# RADIUS 身份验证和 Azure 的多因素身份验证服务器

RADIUS 身份验证部分允许您启用和配置 RADIUS Azure 多因素身份验证服务器的身份验证。 RADIUS 是接受身份验证的请求以及处理这些请求的标准协议。 Azure 的多因素身份验证服务器充当 RADIUS 服务器和身份验证目标，这可能是为了添加 Azure 多因素身份验证的活动目录 (AD)，LDAP 目录或另一个 RADIUS 服务器，您的 RADIUS 客户端 （例如 VPN 设备） 之间插入。 为 Azure 多因素身份验证起作用，必须配置 Azure 的多因素身份验证服务器，以便它可以使用客户端服务器和身份验证目标进行通信。 Azure 的多因素身份验证服务器接受 RADIUS 客户端的请求，验证凭据的身份验证目标、 添加 Azure 多因素身份验证，发送回 RADIUS 客户端的响应。 只有当整个身份验证将会成功首要身份验证和 Azure 多因素身份验证成功。

![Radius 身份验证](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## RADIUS 身份验证配置

要配置 RADIUS 身份验证，请在 Windows 服务器上安装 Azure 的多因素身份验证服务器。 如果 Active Directory 环境，服务器应该加入到网络中的域。 使用以下过程将 Azure 的多因素身份验证服务器配置︰ 

1. 在 Azure 多因素身份验证服务器内单击左侧菜单中的 RADIUS 身份验证图标。
2. 选中启用 RADIUS 身份验证复选框。
3. 在客户端选项卡上更改身份验证端口和记帐端口如果 Azure 多因素身份验证 RADIUS 服务应绑定到非标准端口以侦听来自客户端的配置 RADIUS 请求。
4. 单击添加。. 按钮。
5. 在添加 RADIUS 客户端对话框中，输入 IP 地址的装置/服务器将进行身份验证的 Azure 的多因素身份验证服务器、 应用程序名称 （可选） 和共享机密。 需要是 Azure 多因素身份验证服务器和装置/服务器上相同的共享的机密。 应用程序名称在 Azure 多因素身份验证报告中出现，并可能显示 SMS 或移动应用程序的身份验证消息内。
6. 如果所有用户已经执行或将导入到服务器和受多因素身份验证，请选中需要多因素身份验证的用户匹配。 如果大量用户不还已导入到服务器和/或将会受多因素身份验证，请取消选中框。 此功能，请参阅帮助文件的其他信息。
7. 如果用户将使用 Azure 多因素身份验证移动应用程序身份验证并且想要使用备用身份验证与带外电话联络、 SMS 或推式通知誓言密码，请选中启用回退誓言标记框。
8. 单击确定按钮。
9. 您可以重复步骤 4 至 8 以添加其他 RADIUS 客户端。
10. 单击目标选项卡。
11. 如果在活动目录环境中加入域的服务器上安装了 Azure 的多因素身份验证服务器，选择 Windows 域。
12. 如果用户应根据 LDAP 目录身份验证，请选择 LDAP 绑定。 在使用 LDAP 绑定时，必须单击目录集成图标并编辑设置选项卡上的 LDAP 配置，以便服务器可以将绑定到您的目录。 在 LDAP 代理服务器配置手册 》 中找不到配置 LDAP 的说明。 
13. 如果用户应针对另一个 RADIUS 服务器进行身份验证，请选择 RADIUS 服务器。
14. 配置服务器，该服务器将代理到 RADIUS 请求通过单击添加... 按钮。
15. 在添加 RADIUS 服务器对话框中输入 RADIUS 服务器和共享机密的 IP 地址。 共享的密钥需要的 Azure 多因素身份验证服务器和 RADIUS 服务器上相同。 如果所使用的 RADIUS 服务器的不同端口，更改身份验证和记帐端口。
16. 单击确定按钮。 
17. 必须作为中另一个 RADIUS 服务器的 RADIUS 客户端添加 Azure 的多因素身份验证服务器，以便它将处理从 Azure 的多因素身份验证服务器发送给它的访问请求。 您必须使用相同的共享的机密在 Azure 的多因素身份验证服务器配置。
18. 您可以重复此步骤以添加其他 RADIUS 服务器和配置的服务器应该给他们打电话的移动和下移按钮的顺序。 这就完成了 Azure 多因素身份验证服务器配置。 服务器正在侦听的已配置端口上配置的客户端从 RADIUS 访问请求。   


## RADIUS 客户端配置

若要配置 RADIUS 客户端，使用指南︰

- 配置装置/服务器以通过半径通过 Azure 多因素身份验证服务器的 IP 地址，将充当 RADIUS 服务器的身份验证。 
- 使用上面配置的共享的机密。 
- 配置 RADIUS 超时设置为 30-60 秒，以便验证用户的凭据执行多因素身份验证，收到其答复，然后对 RADIUS 访问请求作出响应的时间。

