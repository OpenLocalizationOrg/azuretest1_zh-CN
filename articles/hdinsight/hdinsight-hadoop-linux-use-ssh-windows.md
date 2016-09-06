---
ms.openlocfilehash: 864178cd0ec2810b9ad003a147fd42114936ef05
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在从 Windows 的基于 Linux 的群集上使用 Hadoop 的 SSH 密钥 |Microsoft Azure"
   description="了解如何创建和使用 SSH 密钥进行身份验证与基于 Linux 的 HDInsight 群集。 通过使用 PuTTY SSH 客户端从基于 Windows 的客户端连接群集。"
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

#在从 Windows （预览） 的 HDInsight 的基于 Linux 的 Hadoop 使用 SSH

> [AZURE.SELECTOR]
- [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
- [Linux、 Unix，OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

基于 linux * 的 Azure HDInsight 群集提供使用安全外壳协议 (SSH) 访问 SSH 密钥或密码的选项。 本文档提供了有关使用 PuTTY SSH 客户端从基于 Windows 的客户端连接到 HDInsight 的信息。

> [AZURE.NOTE] 这篇文章中的步骤假定您在使用基于 Windows 的客户端。 如果您正在使用 Linux、 Unix 或 OS X 客户端，请参阅[使用 SSH 上从 Linux、 Unix 或 OS X HDInsight 基于 Linux 的 Hadoop 使用](hdinsight-hadoop-linux-use-ssh-unix.md)。

##先决条件

* **PuTTY**和**PuTTYGen**的基于 Windows 的客户端。 这些实用程序可从[http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。

* 现代 web 浏览器支持 HTML5。

运算，在指定数据点中设置指定位数。

* [用于 Mac、 Linux 和 Windows azure CLI](../xplat-cli.md)。

##SSH 是什么？

SSH 是用于登录到，并远程执行命令在远程服务器上的实用程序。 与基于 Linux 的 HDInsight，SSH 建立加密的连接到群集的头节点，并提供了一个用于键入命令的命令行。 然后，直接在服务器上执行命令。

##创建 SSH 密钥 （可选）

在创建一个基于 Linux 的 HDInsight 群集时，您可以使用 SSH 密钥或密码身份验证到服务器时使用 SSH。 被视为更安全，SSH 密钥，因为它们是基于证书的。 如果您计划在群集中使用 SSH 密钥，请使用下面的信息。

1. 打开 PuTTYGen。

2. 对于**生成的密钥类型**，选择**SSH 2 RSA**，，然后单击**生成**。

    ![PuTTYGen 接口](./media/hdinsight-hadoop-linux-use-ssh-windows/puttygen.png)

3. 移动鼠标，在进度栏下的区域直到栏填满。 移动鼠标生成用于生成密钥的随机数据。

    ![移动鼠标](./media/hdinsight-hadoop-linux-use-ssh-windows/movingmouse.png)

    一旦生成的密钥，将显示中的公共密钥。

4. 为了提高安全性，可以在**密钥密码**字段中，输入密码，然后在**确认密码**字段中键入相同的值。

    ![密码短语](./media/hdinsight-hadoop-linux-use-ssh-windows/key.png)

    > [AZURE.NOTE] 我们强烈建议您的密钥使用安全密码。 但是，如果您忘记了密码，则无法恢复它。

5. 单击**保存专用密钥** **.ppk**文件中保存密钥。 该密钥将用于通过身份验证的基于 Linux 的 HDInsight 群集。

    > [AZURE.NOTE] 因为可以用它来访问您的基于 Linux 的 HDInsight 群集应该存储在一个安全位置，此注册表项。

6. 单击**保存公钥**将该项保存为**.txt**文件。 这使您可以创建其他基于 Linux 的 HDInsight 群集时将来重复使用的公钥。

    > [AZURE.NOTE] 顶部的 PuTTYGen 还显示中的公共密钥。 您可以用鼠标右键单击该字段、 复制值，然后将其粘贴到一个窗体，当创建群集使用 Azure 预览门户。

##创建一个基于 Linux 的 HDInsight 群集

在创建一个基于 Linux 的 HDInsight 群集时，您必须提供以前创建的公用密钥。 从基于 Windows 的客户端有两种方法可以创建一个基于 Linux 的 HDInsight 群集︰

* **Azure 预览门户**-使用基于 web 的门户网站来创建群集。

* **用于 Mac、 Linux 和 Windows azure CLI** -使用命令行命令来创建群集。

每种方法都需要公共密钥。 有关创建基于 Linux 的 HDInsight 群集的完整信息，请参阅[设置 Linux 基于 HDInsight 群集](hdinsight-hadoop-provision-linux-clusters.md)。

###Azure 预览门户

当使用[Azure 预览门户网站][预览门户]创建一个基于 Linux 的 HDInsight 群集，您必须输入**SSH 用户名**，并选择输入**密码**或**SSH 公钥**。

如果选择**SSH 公用密钥**，也可以粘贴公钥 (显示在__以便粘贴到授权的 OpenSSH 的公钥\_密钥文件__字段中 PuttyGen，) 到__SSH PublicKey__字段，或选择__选择一个文件__，浏览并选择包含公钥的文件。

![询问公共密钥的窗体的图像](./media/hdinsight-hadoop-linux-use-ssh-windows/ssh-key.png)

这将创建指定的用户的登录名并启用密码身份验证或 SSH 密钥身份验证。

###用于 Mac、 Linux、 Windows azure 的命令行界面

您可以使用[Mac、 Linux 和 Windows Azure CLI](../xplat-cli.md)创建新群集使用`azure hdinsight cluster create`命令。

使用此命令的详细信息，请参阅[配置 Hadoop Linux 群集中使用的自定义选项的 HDInsight](hdinsight-hadoop-provision-linux-clusters.md)。

##连接到基于 Linux 的 HDInsight 群集

1. 打开 PuTTY。

    ![putty 接口](./media/hdinsight-hadoop-linux-use-ssh-windows/putty.png)

2. 如果您提供 SSH 密钥，创建您的用户帐户时，您必须执行以下步骤，选择对群集进行身份验证时使用的专用密钥︰

    在**类别**中，扩展**连接**，展开**SSH**，选择**身份验证**。 最后，单击**浏览**并选择.ppk 文件，其中包含您的私钥。

    ![putty 界面，选择专用密钥](./media/hdinsight-hadoop-linux-use-ssh-windows/puttykey.png)

3. 在**类别**中，选择**会话**。 从**基本的 PuTTY 会话选项**屏幕，在**主机名 （或 IP 地址）**字段中输入 HDInsight 服务器的 SSH 地址。 SSH 地址为您的群集名称，则**-ssh.azurehdinsight.net**。 例如， **mycluster-ssh.azurehdinsight.net**。

    ![putty 接口使用 ssh 输入地址](./media/hdinsight-hadoop-linux-use-ssh-windows/puttyaddress.png)

4. 要保存连接信息供将来使用，请输入下**保存会话**，此连接的名称，然后单击**保存**。 连接将被添加到已保存会话的列表中。

5. 单击**打开**到群集的连接。

    > [AZURE.NOTE] 如果是第一次连接到群集，您将收到安全警报。 这是正常的。 选择**是**以缓存服务器的 RSA2 键继续。

6. 出现提示时，输入在创建群集时输入的用户。 如果您为用户提供了密码，将提示您重新输入一遍也。

> [AZURE.NOTE] 以上步骤假定您在使用端口 22 日将连接到 headnode0 HDInsight 群集上。 如果您使用端口 23，您将连接到 headnode1 中。 头节点的详细信息，请参阅[HDInsight 中的 Hadoop 群集的可用性和可靠性](hdinsight-high-availability-linux.md)。

###连接到辅助节点

辅助节点不直接可以从外部访问 Azure 数据中心，但他们可以从群集的头节点，通过 SSH 访问。

如果您提供 SSH 密钥，创建您的用户帐户时，您必须执行下列步骤以使用私有密钥，如果您想要连接到辅助节点到群集验证身份时。

1. 从[http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)安装露天表演。 此实用程序用于缓存 PuTTY SSH 密钥。

2. 运行露天表演。 将最大限度地为状态任务栏中的图标。 用鼠标右键单击该图标，然后选择**添加项**。

    ![添加项](./media/hdinsight-hadoop-linux-use-ssh-windows/addkey.png)

3. 浏览对话框出现时，选择.ppk 文件包含密钥，，然后单击**打开**。 这为露天表演，它将其提供给 PuTTY 时连接到群集添加键。

    > [AZURE.IMPORTANT] 如果 SSH 密钥用于保护您的帐户，您必须完成前面的步骤之后，才会能够连接到辅助节点。

4. 打开 PuTTY。

5. 如果您使用 SSH 密钥来进行身份验证，在**类别**部分中，展开**连接**、 展开**SSH**，然后选择**身份验证**。

    在**身份验证参数**部分中，启用**允许代理转发**。 这使得 PuTTY 自动将证书身份验证，通过连接传递给群集的头节点时连接到辅助节点。

    ![允许代理转发](./media/hdinsight-hadoop-linux-use-ssh-windows/allowforwarding.png)

6. 前面所述，请连接到群集。 如果您使用 SSH 密钥身份验证，不需要选择键-添加到露天表演的 SSH 密钥用于验证群集。

7. 在建立连接后，使用以下内容以检索您的群集中的节点的列表。 *ADMINPASSWORD*替换为您的群集管理员帐户的密码。 *群集名称*替换您的群集的名称。

        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts

    这将显示群集中的节点的 JSON 格式返回的信息包括`host_name`，其中包含的每个节点的完全限定的域名 (FQDN)。 下面是一个示例`host_name`**卷曲**命令所返回的条目︰

        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"

8. 一旦您想要连接到辅助节点的列表，使用下面的命令从 PuTTY 会话打开一个连接到辅助节点︰

        ssh USERNAME@FQDN

    替换您的 SSH 用户姓名和*FQDN*与辅助节点的 FQDN*用户名*。 例如， `workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`。

    > [AZURE.NOTE] 如果您使用密码来验证您的 SSH 会话，则将提示您再次输入密码。 如果您使用 SSH 密钥，该连接应该完成没有任何提示。

9. 一旦已经建立的会话，将改为 PuTTY 会话的提示`username@headnode`到`username@workernode`来指示您是否连接到辅助节点。 此时运行的任何命令将在辅助节点上运行。

10. 完成该工作节点上执行操作，使用`exit`命令以关闭到辅助节点会话。 这将返回到您`username@headnode`提示。

##添加更多帐户

如果需要在向群集中添加更多帐户，请执行以下步骤︰

1. 如上文所述，生成一个新的公钥和私钥的新用户帐户。

2. 从群集的 SSH 会话，添加新用户使用以下命令︰

        sudo adduser --disabled-password <username>

    这将创建一个新的用户帐户，但将禁用密码身份验证。

3. 创建目录和文件按住键，使用以下命令︰

        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys

4. Nano 编辑器打开时，在复制和粘贴为新的用户帐户中的公共密钥的内容。 最后，使用**Ctrl-X**若要保存文件并退出编辑器。

    ![nano 编辑器示例项的图像](./media/hdinsight-hadoop-linux-use-ssh-windows/nano.png)

5. 使用下面的命令将.ssh 文件夹及其内容的所有权更改为新的用户帐户︰

        sudo chown -hR <username>:<username> /home/<username>/.ssh

6. 您现在可以对具有新的用户帐户和专用密钥的服务器进行身份验证。

##<a id="tunnel"></a>SSH 隧道

可以使用 SSH 隧道本地请求，如 HDInsight 群集的 web 请求。 如同它有是在 HDInsight 群集的头节点上生成，然后将所请求资源路由请求。

> [AZURE.IMPORTANT] SSH 隧道是访问 sopme Hadoop 服务 web 用户界面的要求。 例如，该作业历史记录用户界面或资源管理器用户界面只可以使用 SSH 隧道。

使用以下步骤创建 SSH 隧道和配置浏览器以使用它来连接到群集︰

1. 打开 PuTTY，然后输入连接信息，如前面的节介绍了[连接到一个基于 Linux 的群集](#connect-to-a-linux-based-hdinsight-cluster)。

2. 在对话框左侧的**类别**部分中，展开**连接**， **SSH**，展开，然后选择**隧道**。

3. 在**控制 SSH 端口转发选项**窗体中提供以下信息︰

    * **源端口**--您希望转发客户端上的端口。 例如，为**9876**。

    * **目标**-SSH 基于 Linux 的 HDInsight 群集地址。 例如， **mycluster-ssh.azurehdinsight.net**。

    * **动态**-启用动态路由的 SOCKS 代理。

    ![图像的隧道选项](./media/hdinsight-hadoop-linux-use-ssh-windows/puttytunnel.png)

4. 单击**添加**以添加设置，然后再单击**打开**来打开 SSH 连接。

5. 如果出现提示，请登录到服务器。 这会建立 SSH 会话并启用隧道。

6. 配置客户端程序，例如 Firefox，将**localhost:9876**用作**SOCKS v5**代理。 以下是 Firefox 设置如下所示︰

    ![Firefox 设置的图像](./media/hdinsight-hadoop-linux-use-ssh-windows/socks.png)

    > [AZURE.NOTE] 选择**远程 DNS**将使用 HDInsight 群集中解决 DNS 请求。 如果未选中，则将本地解析 DNS。

    您可以验证，由 vising [http://www.whatismyip.com/](http://www.whatismyip.com/)等网站启用和禁用在 Firefox 中的代理设置路由通过隧道通信。 而启用，IP 地址将在 Microsoft Azure 数据中心机。

###浏览器扩展

配置浏览器以使用该隧道的工作方式，而通常不想要通过隧道路由所有通信。 如[FoxyProxy](http://getfoxyproxy.org/)浏览器扩展支持模式匹配的 URL 请求 (FoxyProxy 标准或加上只)，因此，只对特定 Url 的请求将被发送通过隧道。

如果您已经安装了 FoxyProxy 标准，使用以下步骤将其配置为仅为 HDInsight 通过隧道转发通信。

1. 在浏览器中打开 FoxyProxy 扩展名。 例如，在 Firefox 中，选择地址字段旁边的 FoxyProxy 图标。

    ![foxyproxy 图标](./media/hdinsight-hadoop-linux-use-ssh-windows/foxyproxy.png)

2. 选择**添加新的代理服务器**，选择**常规**选项卡，然后输入代理服务器名称的**HDInsightProxy**。

    ![常规的 foxyproxy](./media/hdinsight-hadoop-linux-use-ssh-windows/foxygeneral.png)

3. 选择**代理详细信息**选项卡，并填写以下字段︰

    * **主机或 IP 地址**-这是本地主机，因为我们在本地机器上使用 SSH 隧道。

    * **端口**--这是 SSH 隧道使用的端口。

    * **SOCKS 代理服务器**-选择此选项可启用浏览器代理服务器作为使用该隧道。

    * **SOCKS v5** -选择此选项可设置为代理服务器所需的版本。

    ![foxyproxy 代理](./media/hdinsight-hadoop-linux-use-ssh-windows/foxyproxyproxy.png)

4. 选择**URL 模式**选项卡，然后选择**添加新模式**。 使用以下方法来定义该模式中，并单击**确定**:

    * **图案名称** - **headnode** -这是只是该模式的友好名称。

    * **URL 模式** - **\*headnode\* ** -这将定义与任何 word **headnode**中它与 URL 匹配的模式。

    ![foxyproxy 模式](./media/hdinsight-hadoop-linux-use-ssh-windows/foxypattern.png)

4. 单击**确定**以添加代理并关闭**代理服务器设置**。

5. 在 FoxyProxy 对话框的顶部，**选择模式**更改为**使用代理服务器根据预定义的模式和优先级**，，然后单击**关闭**。

    ![foxyproxy 选择模式](./media/hdinsight-hadoop-linux-use-ssh-windows/selectmode.png)

执行以下步骤后, 只包含字符串**headnode**的 Url 的请求将通过 SSL 隧道路由。

##下一步行动

现在，您将了解如何使用 SSH 密钥进行身份验证，了解如何通过在 HDInsight 上的 Hadoop 使用 MapReduce。

* [使用 HDInsight 配置单元](hdinsight-use-hive.md)

* [使用 HDInsight 的小猪](hdinsight-use-pig.md)

* [HDInsight 使用 MapReduce 作业](hdinsight-use-mapreduce.md)

[预览门户]: https://portal.azure.com/
测试t
