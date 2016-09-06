---
ms.openlocfilehash: 7a47c51e263f7f8c441b6d34e790772a02cb9372
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在虚拟机上的 tomcat |Microsoft Azure"
    description="了解如何创建 Windows 虚拟机并将计算机运行 Apache Tomcat 应用程序服务器配置。"
    services="virtual-machines"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="virtual-machines"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="06/03/2015"
    ms.author="robmcm"/>

# 如何在虚拟机上运行 Java 应用程序服务器

使用 Azure，可以使用虚拟机来提供服务器功能。 举一个例子，可以配置在 Azure 上运行的虚拟机来维护 Java 应用程序服务器，如 Apache Tomcat。 完成本指南后，您会了解如何创建在 Azure 上运行的虚拟机并将其配置为运行 Java 应用程序服务器。

您将了解︰

* 如何创建虚拟机都已安装了 Java 开发工具箱 (JDK)。
* 如何远程登录到您的虚拟机。
* 如何在虚拟机上安装的 Java 应用程序服务器。
* 如何创建终结点，您的虚拟机。
* 如何为您的应用程序服务器防火墙中打开端口。

出于本教程的目的，将在虚拟机上安装 Apache Tomcat 应用程序服务器。 已完成的安装，将导致如下所示的 Tomcat 安装。

![虚拟机运行 Apache Tomcat][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## 若要创建虚拟机

1. 登录到[Azure 的门户](https://manage.windowsazure.com)。
2. 单击**新建****计算**单击**虚拟机**，和，然后单击**剪辑库中**。
3. 在**虚拟机映像选择**对话框中，选择**JDK 7 Windows Server 2012**。
请注意，是否您有没有准备好运行 JDK 7 中的旧版应用程序可用**JDK 6 Windows Server 2012** 。
4. 单击**下一步**。
5. 在**虚拟机配置**对话框中︰
    1. 指定虚拟机的名称。
    2. 指定要为该虚拟机使用的大小。
    3. 在**用户名**字段中输入管理员的名称。 请记住此名称，并且您将在下一步输入的密码，您可以使用它们，如果您远程登录到虚拟机。
    4. 在**新密码**字段中，输入密码，在**确认**字段中重新输入。 这是管理员帐户密码。
    5. 单击**下一步**。
6. 在下一步的**虚拟机配置**对话框中︰
    1. **云服务**，使用默认设置**创建新的云服务**。
    2. Cloudapp.net**云服务 DNS 名称**的值必须唯一。 如果需要请修改此值，所以该 Azure 指示它是唯一的。
    2. 指定某个地区、 相似性组或虚拟网络。 对于本教程，请指定如**美国西部**地区。
    2. 对于**存储帐户**，请选中**使用自动生成的存储帐户**。
    3. 对于**可用性设置**，请选择**（无）**。
    4. 单击**下一步**。
7. 最终在**虚拟机配置**对话框中︰
    1. 接受默认终结点项。
    2. 单击**完成**。

## 远程登录到您的虚拟机

1. 登录到[管理门户](https://manage.windowsazure.com)。
2. 单击**虚拟机**。
3. 单击想要登录到虚拟机的名称。
4. 虚拟机启动后，在页面底部的弹出式菜单允许连接。
5. 单击**连接**。
6. 对提示作出响应，根据需要连接到虚拟机。 这应该需要保存或打开.rdp 文件，其中包含连接的详细信息。 您可能需要为.rdp 文件的第一行的最后一个部分复制 url︰ 端口并将其粘贴在远程登录应用程序。

## 若要在虚拟机上安装 Java 应用程序服务器

可以将 Java 应用程序服务器复制到您的虚拟机，或者您可以安装到安装程序的 Java 应用程序服务器。

出于本教程中，将安装 Tomcat。

1. 当您登录到您的虚拟机，请打开与[Apache Tomcat](http://tomcat.apache.org/download-70.cgi)的浏览器会话。
2. 双击**32 位 64 位 Windows 服务**安装程序链接。 通过使用此技术，将被 Tomcat 安装作为 Windows 服务。
3. 出现提示时，选择要运行安装程序。
4. 在**Apache Tomcat 安装**向导中，请按照提示安装 Tomcat。 出于本教程中，接受默认设置是可以的。 当达到**完成 Apache Tomcat 安装向导**对话框中时，您可以 （可选） 检查**运行 Apache Tomcat**能够立即启动 Tomcat。 单击**完成**以完成 Tomcat 安装过程。

## 若要启动 Tomcat
如果您未选择运行 Tomcat**完成 Apache Tomcat 安装向导**对话框中，请打开您的虚拟机上的命令提示符并运行**网络启动 Tomcat7**启动它。

您现在应该看到运行如果您在运行虚拟机的浏览器并打开<http://localhost:8080>Tomcat。

若要查看从外部计算机运行的 Tomcat，您需要创建一个终结点，并打开一个端口。

## 若要创建虚拟机的终结点
1. 登录到[管理门户](https://manage.windowsazure.com)。
2. 单击**虚拟机**。
3. 单击运行 Java 应用程序服务器虚拟机的名称。
4. 单击**终结点**。
5. 单击**添加**。
6. 在**添加终结点**对话框中，确保**添加独立端点**被选中，然后单击**下一步**。
7. 在**新的终结点详细信息**对话框中︰
    1. 指定终结点;例如， **HttpIn**。
    2. 指定**TCP**协议。
    3. 指定的公用端口**80** 。
    4. 指定为专用端口**8080** 。
    5. 单击**完成**按钮关闭该对话框。 现在将创建终结点。

## 若要打开某个端口在防火墙中为您的虚拟机
1. 登录到您的虚拟机。
2. 单击**开始窗口**。
3. 单击**控制面板**。
4. 单击**系统和安全**，单击**Windows 防火墙**，然后单击**高级设置**。
5. **入站规则**，请单击，然后单击**新建规则**。
 ![新入站的规则][NewIBRule]
6. **规则类型**，选择**端口**，然后单击**下一步**。
 ![新入站的规则端口][NewRulePort]
7. **协议和端口**在屏幕上，选择**TCP**，指定**特定的本地端口** **8080** ，然后单击**下一步**。
 ![新入站的规则 ][NewRuleProtocol]
8. 在**操作**屏幕中，选择**允许连接**，然后单击**下一步**。
 ![新入站的规则操作][NewRuleAction]
9. 在**配置文件**屏幕中，确保**域**、**专用**和**公用**都被选中，然后单击**下一步**。
 ![新入站的规则的配置文件][NewRuleProfile]
10. 在**名称**屏幕中，为规则，如**HttpIn** （规则名称不需要匹配的终结点名称，但是），指定一个名称，然后单击**完成**。  
 ![新入站的规则名称][NewRuleName]

此时，Tomcat 网站应从外部浏览器可以查看通过使用窗体的 URL * *http://*您\_DNS\_名称*。 cloudapp.net**，其中***您\_DNS\_名称** * 是虚拟机创建时所指定的 DNS 名称。

## 应用程序生命周期的注意事项
* 您可以创建您自己的 web 应用程序归档 （战争） 并将其添加到**webapps**文件夹。 例如，创建一个基本的 Java 服务页 (JSP) 动态 web 项目并将其导出为一个 WAR 文件，复制到虚拟机，然后在浏览器中运行其上的 Apache Tomcat **webapps**文件夹的战争。
* 默认情况下安装 Tomcat 服务时，它被设置为手动启动。 您可以切换为服务管理单元中使用自动启动。 启动服务管理单元中的**Windows 启动**、**管理工具**，单击，然后单击**服务**。 双击**Apache Tomcat**的服务，将**启动类型**设置为**自动**。

    ![将服务设置为自动启动][service_automatic_startup]

    自动拥有 Tomcat 起始的好处是如果虚拟机重新启动时 （例如，在安装了软件更新需要重新启动） 就会启动。

## 下一步行动
了解其他服务 （如 Azure 存储、 服务总线和 SQL 数据库），您可能需要包括与 Java 应用程序通过在[Java 开发人员中心](http://azure.microsoft.com/develop/java/)中查看可用的信息。

[virtual_machine_tomcat]: ./media/virtual-machines-java-run-tomcat-application-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-java-run-tomcat-application-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-java-run-tomcat-application-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-java-run-tomcat-application-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleProfile.png
