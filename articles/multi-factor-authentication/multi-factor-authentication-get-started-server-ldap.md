---
ms.openlocfilehash: 6c2b2654aacc33ab679d1445ab9cdb3874102e8f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="LDAP 身份验证和 Azure 的多因素身份验证服务器" 
    description="这是部署 LDAP 身份验证和 Azure 多因素身份验证服务器将协助 Azure 的多因素身份验证页。" 
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

# LDAP 身份验证和 Azure 的多因素身份验证服务器 


默认情况下，Azure 的多因素身份验证服务器配置为导入或同步 Active Directory 中的用户。 但是，它可以配置要绑定到不同的 LDAP 目录，如 ADAM 目录或特定 Active Directory 域控制器。 当配置为连接到通过 LDAP 目录，可以配置 Azure 的多因素身份验证服务器充当一个 LDAP 代理来执行身份验证。 它还允许使用的 LDAP 绑定作为半径的目标，为预身份验证的用户在使用 IIS 身份验证，或在 Azure 多因素身份验证的用户门户的首要身份验证。

时为 LDAP 代理使用 Azure 多因素身份验证，Azure 的多因素身份验证服务器 LDAP 客户端 （例如 VPN 设备、 应用程序） 和 LDAP 目录服务器之间插入要添加多因素身份验证。 对于 Azure 函数的多因素身份验证，必须配置 Azure 的多因素身份验证服务器进行通信的客户端服务器和 LDAP 目录。 在此配置中，Azure 的多因素身份验证服务器接受来自客户端的服务器和应用程序的 LDAP 请求，并将它们转发到目标 LDAP 目录服务器，以验证主要的凭据。 如果 LDAP 目录中的响应显示它们主要的凭据是有效的 Azure 多因素身份验证执行第二个双因素身份验证，并发送到 LDAP 客户端的响应。 只有当到 LDAP 服务器的身份验证和多因素身份验证成功，整个身份验证将会成功。 





## LDAP 身份验证配置


要配置 LDAP 身份验证，请在 Windows 服务器上安装 Azure 的多因素身份验证服务器。 请执行以下步骤︰ 

1. Azure 的多因素身份验证服务器内单击左侧菜单中的 LDAP 身份验证图标。
2. 选中启用 LDAP 身份验证复选框。![LDAP 身份验证](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png) 
3. 在客户端选项卡上更改 TCP 端口与 SSL 端口如果 Azure 多因素身份验证 LDAP 服务应绑定到非标准端口以侦听来自客户端的配置的 LDAP 请求。
4. 如果您打算到 Azure 多因素身份验证服务器从客户端使用 LDAPS，必须在服务器运行的服务器上安装 SSL 证书。 单击浏览按钮。. SSL 证书框旁边的按钮，然后选择将使用安全连接的已安装的证书。 
5. 单击添加。. 按钮。
6. 在添加 LDAP 客户机对话框中，输入 IP 地址的装置、 服务器或应用程序将身份验证服务器和应用程序名称 （可选）。 应用程序名称在 Azure 多因素身份验证报告中出现，并可能显示 SMS 或移动应用程序的身份验证消息内。
7. 如果所有用户已经执行或将导入到服务器和受多因素身份验证，请选中需要 Azure 多因素身份验证的用户匹配。 如果大量用户不还已导入到服务器和/或将会受多因素身份验证，请取消选中框。 此功能，请参阅帮助文件的其他信息。 
8. 您可以重复步骤 5 至 7，以添加其他 LDAP 客户端。
9. Azure 的多因素身份验证配置为接收 LDAP 身份验证，则它必须代理到 LDAP 目录的身份验证。 因此，单一的目标选项卡仅显示灰色使用 LDAP 目标选项。 若要配置的 LDAP 目录连接，请单击目录集成图标。 
10. 在设置选项卡上，选择使用特定 LDAP 配置单选按钮。
11. 单击编辑命令。. 按钮。
12. 在编辑 LDAP 配置对话框中连接到 LDAP 目录所需的信息填充的字段。 下表中包含字段的说明。 注︰ 在 Azure 多因素身份验证服务器帮助文件还包含此信息。![目录集成](./media/multi-factor-authentication-get-started-server-ldap/ldap.png) 
13. 通过单击测试按钮测试的 LDAP 连接。
14. 如果 LDAP 连接测试成功，请单击确定按钮。 
15. 单击筛选器选项卡。 服务器已预配置为从 Active Directory 中加载容器、 安全组和用户。 如果绑定到另一个 LDAP 目录，您可能需要编辑显示筛选器。 单击筛选器的详细信息的帮助链接。
16. 单击属性选项卡。 服务器已预配置为从活动目录的属性映射。
17. 如果绑定到另一个 LDAP 目录，或更改预先配置的属性映射，单击编辑命令。. 按钮。
18. 在编辑属性对话框中，修改您的目录的 LDAP 属性映射。 可以键入或选择通过单击属性名称... 每个字段旁边的按钮。
19. 单击属性的详细信息的帮助链接。
20. 单击确定按钮。
21. 单击公司设置图标并选择用户名解析选项卡。
22.如果从加入域的服务器连接到活动目录，可以继续使用 Windows 安全标识符 (Sid) 匹配的用户名单选按钮处于选中状态。 否则，请选择匹配的用户名单选按钮的使用 LDAP 唯一标识符属性。 当选中时，Azure 的多因素身份验证服务器将尝试将每个用户名解析为 LDAP 目录中的唯一标识符。 将执行 LDAP 搜索用户名属性定义在目录集成-> 属性选项卡。 当用户进行身份验证时，该用户名将解析为 LDAP 目录中的唯一标识符并将匹配的 Azure 多因素身份验证数据文件中的用户使用的唯一标识符。 这样，对于不区分大小写的比较，也为长格式和短的用户名格式这完成 Azure 多因素身份验证服务器配置。 服务器正在侦听的已配置端口上配置的客户端，从 LDAP 访问请求，并将这些请求到 LDAP 目录的身份验证设置为代理。


## LDAP 客户端配置

若要配置 LDAP 客户端，使用指南︰

- 配置装置、 服务器或应用程序，就好像它是 LDAP 目录通过到 Azure 多因素身份验证服务器的 LDAP 身份验证。 您应使用相同的设置，通常使用直接连接到您的 LDAP 目录，除将 Azure 多因素身份验证服务器的 IP 地址的服务器名称。 
- 配置 LDAP 超时设置为 30-60 秒，以便验证用户的凭据与 LDAP 目录执行第二个双因素身份验证，收到其答复，然后对 LDAP 访问请求作出响应的时间。 
- 如果使用 LDAPS，装置或进行 LDAP 查询的服务器必须信任 Azure 多因素身份验证服务器上安装 SSL 证书。

