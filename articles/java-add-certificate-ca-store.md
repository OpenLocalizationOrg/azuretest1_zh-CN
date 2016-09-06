---
ms.openlocfilehash: cf737001162dc49baf7e2179da03b56d13a61fd7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将证书添加到 Java CA 存储 |Microsoft Azure" 
    description="了解如何将证书颁发机构 (CA) 证书添加到 Java CA 证书 (cacerts) 存储为 Twilio 服务或 Azure 服务总线。" 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/31/2015" 
    ms.author="robmcm"/>

# Java 的 CA 的证书存储区中添加证书
以下步骤显示了如何将证书颁发机构 (CA) 证书添加到 Java CA 证书 (cacerts) 存储。 使用示例是 Twilio 服务所需的 CA 证书。 后面的主题中提供的信息介绍如何安装此 CA 证书的 Azure 服务总线。 

您可以使用 keytool 添加 CA 证书，然后再压缩您的 JDK 和将其添加到 Azure 项目的**approot**文件夹中，或者您无法运行使用 keytool 添加证书 Azure 启动任务。 此示例假定您将添加之前被压缩的 JDK 中的 CA 证书。 此外，在示例中，将使用特定 CA 证书，但获取不同的 CA 证书并将其导入 cacerts 存储的步骤将是类似。

## 若要将证书添加到 cacerts 存储

1. 在 JDK 的**jdk\jre\lib\security**文件夹设置的命令提示符下运行以下内容来查看安装的证书︰

    `keytool -list -keystore cacerts`

    系统将提示您存储密码。 默认密码被**修改**。 （如果您想要更改密码，请参阅 keytool 文档，网址<http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>。此示例假定该证书的 MD5 指纹 67:CB:9 D︰ 未列出 C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4，并且您想要导入 （Twilio API 服务需要该特定证书）。
2. 从[GeoTrust 的根证书](http://www.geotrust.com/resources/root-certificates/)中列出的证书的列表获取证书。 用鼠标右键单击证书序列号 35:DE:F4:CF 的链接并将其保存到**jdk\jre\lib\security**文件夹。 对于本示例而言，保存到名为的文件**Equifax\_安全\_证书\_Authority.cer**。
3. 通过下面的命令将证书导入︰

    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`

    如果系统提示您信任该证书，该证书的 MD5 指纹 67:CB:9 D 时︰ C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4，通过键入**y**的响应。
4. 运行以下命令，以确保已成功导入 CA 证书︰

    `keytool -list -keystore cacerts`

5. Zip JDK 并将其添加到 Azure 项目的**approot**文件夹。

Keytool 有关的信息，请参阅<http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>。

## Azure 的根证书

您的应用程序使用 Azure 服务 （如 Azure 服务总线） 需要信任的巴尔的摩 CyberTrust 根证书。 （开始 2013 年 4 月 15 日，Azure 开始从 GTE CyberTrust 全局根迁移到巴尔的摩 CyberTrust 根。 这种迁移花费几个月才能完成）。

证书可能已安装在您的 cacerts 商店，巴尔的摩因此记住要运行**keytool-列表**命令，查看是否已存在。

如果您需要添加巴尔的摩 CyberTrust 根，它具有序列号 02:00:00:b9 和 SHA1 指纹 d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74。 它可以从<https://cacert.omniroot.com/bc2025.crt>，保存为具有扩展名**.cer**，本地文件，然后导入使用**keytool** ，如上所示。

有关使用 Azure 的根证书的详细信息，请参阅[Azure 根证书迁移](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx)。

测试
