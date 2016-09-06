---
ms.openlocfilehash: 9b21bf590f5b1c45f81df0240dbf3a3f5d396bc4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="使用 Ambari 的 HDInsight 群集管理 |Microsoft Azure"
   description="了解如何使用 Ambari 来监控和管理基于 Linux 的 HDInsight 群集。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/24/2015"
   ms.author="larryfr"/>

#通过使用 Ambari （预览） 管理 HDInsight 群集

了解如何使用 Ambari 来管理和监视基于 Linux 的 Azure HDInsight 群集。

> [AZURE.NOTE] 这篇文章中的信息大部分仅适用于基于 Linux 的 HDInsight 群集。 对于基于 Windows 的 HDInsight 群集，只有 Ambari REST API，通过监视才可用。 请参阅[HDInsight 使用 Ambari API 上的监视窗口基于 Hadoop](hdinsight-monitor-use-ambari-api.md)。

##<a id="whatis"></a>Ambari 是什么？

<a href="http://ambari.apache.org" target="_blank">Apache Ambari</a>使 Hadoop 管理通过提供易于使用的 web 用户界面，用于配置、 管理和监视 Hadoop 群集更简单。 开发人员可以将这些功能到其应用程序集成使用<a href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md" target="_blank">Ambari REST Api</a>。

默认情况下，使用基于 Linux 的 HDInsight 群集提供了 Ambari。 基于 Windows 的 HDInsight 群集提供了通过 Ambari REST Api 的监控功能。

##SSH 代理

> [AZURE.NOTE] Ambari 为群集时可以访问，直接通过互联网，从 Ambari Web 用户界面 （如 JobTracker，） 某些链接不在 internet 上公开。 因此，尝试访问这些功能，除非您使用安全外壳协议 (SSH) 隧道代理 web 通信到群集的头节点时，您将收到"服务器未找到"错误。

使用下列文章对群集从您的本地计算机上的端口创建 SSH 隧道︰

* <a href="../hdinsight-hadoop-linux-use-ssh-unix/#tunnel" target="_blank">SSH 使用 HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop</a>的步骤创建一个 SSH 隧道通过`ssh`命令。

* <a href="../hdinsight-hadoop-linux-use-ssh-windows/#tunnel" target="_blank">SSH 使用 HDInsight 从 Windows 上的基于 Linux 的 Hadoop</a> -使用 PuTTY SSH 隧道创建的步骤。

##Ambari web 用户界面

Ambari web 用户界面可在创建每个基于 Linux 的 HDInsight 群集上**https://&lt;群集名称 >。 azurehdinsight.net**。

系统将提示您进行两次身份验证到页面。 第一个提示是通过身份验证的 HDInsight 群集，而第二个是对 Ambari 进行身份验证。

* **群集验证**的使用 （默认值是**管理员**） 的群集管理员用户名和密码。

* **Ambari 身份验证**的用户名和密码的默认值是**管理员**。

    > [AZURE.NOTE] 如果您更改了**管理员**用户的密码，您必须输入新的密码。

当网页打开时，请注意在顶部的栏。 此文件中包含下列信息和控件︰

![ambari 导航](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* **Ambari 徽标**-打开控制板，可用于监视群集。

* **群集名称 # ops** -显示 Ambari 的日常操作的数目。 选择群集名称或**# ops**将显示后台操作的列表。

* **# 警报**-警告或严重警报，如果有的话，该群集。 如果选择此将显示警报的列表。

* **仪表板**的显示仪表板。

* **服务**-在群集中的服务的信息和配置设置。

* **主机**的群集中的节点的信息和配置设置。

* **警报**的信息、 警告和严重警告的日志。

* **管理**-软件堆栈/安装或服务可以添加到群集服务的帐户信息，Kerberos 安全。

* **Admin 按钮**-Ambari 管理、 用户设置和注销。

##监视

###通知

Ambari 提供了很多警报，将具有下列状态之一︰

* **确定**

* **警告**

* **关键**

* **UNKNOWN**

未准备**好**的警报将导致**# 警报**条目顶部的页后，可以显示的警报数量。 选择此项，将显示警报和它们的状态。

预警分为几个默认组，可以从**通知**页中查看。

![通知页](./media/hdinsight-hadoop-manage-ambari/alerts.png)

您可以通过使用**动作**菜单并选择**管理警报组**管理的组。 这使您可以修改现有的组，或创建新组。

![管理警报分组对话框](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

您还可以从**操作**菜单创建警报通知。 这允许您创建特定的警报严重性组合发生时发送通知通过**电子邮件**或**SNMP**的触发器。 例如，您可以发送警报当任何**YARN 默认**组中的警报设置为**严重**。

![创建警报对话框](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

###群集

仪表板**标准**选项卡包含一系列的构件，方便地监视一眼群集的状态。 几个小部件，如**CPU 使用率**，提供其他的信息，单击时。

![仪表板使用的指标](./media/hdinsight-hadoop-manage-ambari/metrics.png)

**Heatmaps**选项卡将显示为彩色 heatmaps，会从绿色变成红色的指标。

![仪表板使用 heatmaps](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

群集中的节点的详细信息，选择**主机**，然后选择您所感兴趣的特定节点。

![主机详细信息](./media/hdinsight-hadoop-manage-ambari/host-details.png)

###服务

操控板上的**服务**侧栏提供了快速了解群集上运行的服务的状态。 各种图标用于指示状态或应采取的措施，例如，黄色回收符号如果服务需要被回收。

![服务端栏](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

选择服务将在该服务显示更多详细的信息。

![服务的摘要信息](./media/hdinsight-hadoop-manage-ambari/service-details.png)

####快速链接

一些服务在页面的顶部显示的**快速链接**链接。 这可以用来访问特定于服务的 web 用户界面，如︰

* **作业历史记录**的 MapReduce 作业历史记录。

* **资源管理器**-YARN ResourceManager UI。

* **NameNode** -Hadoop 分布式文件系统 (HDFS) NameNode 用户界面。

* **Oozie Web 用户界面**的 Oozie 的用户界面。

选择任何这些链接将打开一个新标签，在浏览器中，它将显示所选的页面。

> [AZURE.NOTE] 选择任何服务一个**快速链接**的链接将导致"找不到服务器"的错误，除非您使用安全套接字层 (SSL) 隧道代理 web 通信到群集。 这是因为 web 应用程序用来显示此信息不会在 internet 上公开。
>
> 在 HDInsight 中使用 SSL 隧道的信息，请参阅下列项之一︰
>
> * <a href="../hdinsight-hadoop-linux-use-ssh-unix/#tunnel" target="_blank">SSH 使用 HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop</a>的步骤创建一个 SSH 隧道通过`ssh`命令。
>
>* <a href="../hdinsight-hadoop-linux-use-ssh-windows/#tunnel" target="_blank">SSH 使用 HDInsight 从 Windows 上的基于 Linux 的 Hadoop</a> -使用 PuTTY SSH 隧道创建的步骤。

##管理

###Ambari 的用户、 组和权限

管理用户、 组和权限不应使用基于 Linux 的 HDInsight 预览期间。

###宿主

**主机**页列出群集中的所有主机。 若要管理主机，请按照下列步骤。

![主机页](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [AZURE.NOTE] 添加、 退役或 recommissioning 主机不应该使用 HDInsight 群集使用。

1. 选择您要管理的主机。

2. 使用**操作**菜单中选择您想要执行的操作︰

    * **启动所有组件**的都启动主机上的所有组件。

    * **停止所有组件**-都停止主机上的所有组件。

    * **重新启动所有组件**-停止和启动主机上的所有组件。

    * **打开维护模式**-主机取消的通知。 如果您执行的操作，将生成警报，如重新启动正在运行的服务依赖的服务，应启用此。

    * **请关闭维护模式**-返回到正常报警主机。

    * **停止**的停止 DataNode 或主机上的 NodeManagers。

    * **开始**-开始 DataNode 或主机上的 NodeManagers。

    * **重新启动**-停止和启动 DataNode 或 NodeManagers 的主机上。

    * **取消**-从群集中删除一个主机。

        > [AZURE.NOTE] 不要在 HDInsight 群集上使用此操作。

    * **Recommission** -添加一个以前已停止使用的主机到群集。

        > [AZURE.NOTE] 不要在 HDInsight 群集上使用此操作。

###<a id="service"></a>服务

从**仪表板**或**服务**页中，使用**操作**按钮底部的服务列表中添加新服务，也可以停止和启动所有服务。

![服务操作](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

添加服务的一般步骤如下︰

1. 从**仪表板**或**服务**页中，使用**操作**按钮，然后选择**添加服务**。

2. 从**添加服务向导**中，选择要添加的服务，然后单击**下一步**。

    ![添加服务](./media/hdinsight-hadoop-manage-ambari/add-service.png)

3. 继续执行向导，提供的服务的配置信息。 配置要求的详细信息，请参阅有关您要添加的特定服务的文档。

4. 从**检查**页中，您可以**打印**的配置信息或**部署**到群集服务。

5. 一旦部署了该服务，**安装，启动和测试**页将显示进度信息，如安装和测试此服务。 绿色**状态**后，选择**下一步**。

    ![映像的安装、 开始时间和测试页](./media/hdinsight-hadoop-manage-ambari/install-start-test.png)

6. **摘要**页显示安装过程中，以及需要采取; 任何可能的操作的摘要例如，重新启动其他服务。 选择**完成**退出向导。

在**操作**按钮可重新启动的所有服务，通常要启动、 停止或重新都启动某个特定的服务。 使用以下步骤来对某一项服务执行操作︰

1. 从**仪表板**或**服务**页中，选择服务。

2. 从**摘要**选项卡上，使用**服务操作**按钮，并选择要执行的操作。 这将重新启动所有节点上的服务。

    ![服务操作](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

    > [AZURE.NOTE] 重新启动某些服务正在运行群集时可能会生成警报。 若要避免此问题，可以使用**服务操作**按钮来执行重新启动之前启用服务的**维护模式**。

3. 一旦选择了某个操作，则会增加页面的顶部**# op**项要显示后台操作正在发生。 如果配置为显示，显示后台操作的列表。

    > [AZURE.NOTE] 如果该服务启用了**维护模式**，请记住一旦操作完成后，通过**服务操作**按钮将其禁用。

若要配置服务，请使用以下步骤︰

1. 从**仪表板**或**服务**页中，选择服务。

2. 选择**配置**选项卡。 将显示当前配置。 此外显示以前的配置的列表。

    ![配置](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. 使用要修改配置，而显示的字段，然后选择**保存**。 或者选择以前的配置，然后选择**成为当前**回滚到以前的设置。

##REST API，

Ambari 网站依赖于基础的 REST API，您可以利用它们来创建您自己的管理和监控工具。 使用相对简单的 API 时，有一些 Azure 的具体情况，需要注意的︰

* 对服务进行身份验证，应使用**身份验证**-群集管理员的用户名 (默认**admin**) 和密码。

* **安全**-Ambari 使用基本身份验证，因此与 API 进行通信时，您总是应该使用安全 HTTP (HTTPS)。

* **IP 地址**— 除非群集是 Azure 的虚拟网络的成员，群集中的主机不是可以从群集外部访问的返回地址。 然后 IP 地址可由其他成员，在虚拟的网络，而不是从外部网络。

* **未启用某些功能**的一些 Ambari 功能被禁用，因为它由 HDInsight 云服务;例如，添加或从群集中删除主机。 其他功能可能不能完全实现在预览时的基于 Linux 的 HDInsight。

REST API 的完整参考，请参阅[Ambari API 参考 V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)。

测试
