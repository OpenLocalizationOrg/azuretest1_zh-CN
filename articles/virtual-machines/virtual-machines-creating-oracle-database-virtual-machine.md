---
ms.openlocfilehash: 855593af170d3161a6627294530441d22f0ab68f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties title="Creating an Oracle Database virtual machine in Azure" pageTitle="在 Azure 创建 Oracle 数据库的虚拟机" description="逐句通过举例说明在 Microsoft Azure 创建 Oracle 的虚拟机，然后在其上创建 Oracle 数据库。" services="virtual-machines" authors="bbenz" documentationCenter=""/>
<tags ms.service="virtual-machines" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="infrastructure-services" ms.date="06/22/2015" ms.author="bbenz" />
#在 Azure 创建 Oracle 数据库的虚拟机
下面的示例演示如何创建基于 Windows Server 2012 在 Azure 上运行 Microsoft 提供 Oracle 数据库映像的虚拟机 (VM)。 有两个步骤。 首先，创建虚拟机，然后再创建 VM 内部 Oracle 数据库。 所示的例子是 Oracle 数据库版本 12 c，但步骤都是为版本 11 g 几乎相同。

##在 Azure 创建 Oracle 数据库的虚拟机

1.  登录到[Azure 的门户](https://ms.portal.azure.com/)。

2.  单击**市场上**单击**计算**，，然后在搜索框中键入**Oracle** 。

3.  选择一个可用的 Oracle 数据库的图像包括**11g 版本、 版本 12 c、 标准版、 企业版或常用的选项或高级的选项的包之一。**  查看有关您选择 （例如，建议最小值），该图像的信息，然后单击**下一步**。

4.  指定虚拟机的**主机名**。

5.  VM 的指定**用户名**。 请注意，此用户是远程登录虚拟机;这不是 Oracle 数据库用户名称。

6.  指定并确认密码用于虚拟机，或提供一个 SSH 公钥。

7.  选择**定价层**。  请注意，默认情况下，若要查看所有的配置选项，单击**所有视图**在右上角显示建议定价层。

8.  根据需要把这些事项设置[可选配置](https://msdn.microsoft.com/library/azure/dn763935.aspx)︰

    一。 保留**存储帐户**-是用虚拟机名称创建新的存储帐户。

    b。 将**可用性设置**保留为"未配置"。

    c。 这一次，不要添加任何**终结点**。

9.  选择或创建资源组。

10. 选择**订阅**。

11. 选择一个**位置**。

12. 单击**创建**，然后创建一个虚拟机的过程将开始。 虚拟机的**运行**状态后，继续执行下一个过程。


##若要创建您的数据库使用 Azure 中的 Oracle 数据库的虚拟机

1.  登录到[Azure 的门户](https://ms.portal.azure.com/)。

2.  单击**虚拟机**。

3.  单击想要登录到虚拟机的名称。

4.  单击**连接**。

5.  对提示作出响应，根据需要连接到虚拟机。 当系统提示您输入管理员名称和密码，使用时创建 VM 所提供的值。

6.  创建名为**ORACLE_HOSTNAME** ，其值设置为虚拟机的计算机名称的环境变量。 您可以创建一个环境变量，使用以下步骤︰

    一。  在 Windows 中，单击**开始**，键入**控制面板**，单击**控制面板**图标、**系统**和安全、 单击**系统**，然后单击**高级的系统设置**。

    b。  单击**高级**选项卡，然后单击**环境变量**。

    c。  在**系统变量**部分中，单击**新建**以创建变量。

    d。  **新建系统变量**对话框中，在**ORACLE_HOSTNAME**输入变量的名称，然后输入虚拟机的计算机名称作为值。 要确定计算机名称，请打开命令提示符并运行**设置计算机名**（该命令的输出将包含计算机名称）。

    电子。  单击**确定**以保存新的环境变量并关闭**新建系统变量**对话框。

    f。  关闭已打开的控制面板的其他对话框。

7.  在 Windows 中，单击**开始**，然后键入**数据库配置的助手**。 单击**数据库配置助手**图标。

8.  在**数据库配置助手**向导中，提供的值，根据需要为每个对话框中的步骤︰

    一。  **第 1 步︰**单击**创建数据库**，然后单击**下一步**。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image5.png)

    b。 **第 2 步︰**输入**全局数据库名称**的值。 输入并确认输入**管理密码**的值。 此密码为您的 Oracle 数据库**系统**用户。 清除**与容器数据库创建**。 单击**下一步**。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image6.png)

    c。 **第 3 步︰**必备项检查时应使用自动前进到**第 4 步**。

    d。 **步骤 4:**检查**创建数据库 – 摘要**选项，并单击**完成**。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image7.png)
    电子。 **第 5 步︰****进度页**将报告数据库创建的状态。

        ![](media/virtual-machines-creating-oracle-database-virtual-machine/image8.png)
    f。 在创建数据库之后，您必须使用**密码管理**对话框中的选项。 修改密码设置，如果需要为您的需求，然后关闭退出**数据库配置助手**向导对话框。

##若要确认您的数据库已安装

1.  仍登录到您的虚拟机，启动 SQL Plus 命令提示符。 在 Windows 中，请单击*开始**，然后键入* *SQL 以及**。单击* *SQL Plus** 图标。

2.  出现提示时，使用**系统**和创建 Oracle 数据库时指定的密码的用户名登录。

3.  在 SQL Plus 命令提示符下运行以下命令。

        **select \* from GLOBAL\_NAME;**

    结果应该是数据库的您创建的全局名称。

    ![](media/virtual-machines-creating-oracle-database-virtual-machine/image9.png)

##允许远程访问您的数据库
允许您远程访问 （例如，从客户端计算机运行 Java 代码） 的数据库，您将需要启动数据库侦听程序，虚拟机的防火墙中打开端口 1521年并创建 1521 公共端点。

### 启动数据库侦听程序
1.  登录到您的虚拟机中。

2.  打开命令提示符。

3.  在命令提示符下，运行下面的命令。

        **lsnrctl start**

> [AZURE.NOTE] 您可以运行**lsnrctl 状态**检查状态的侦听器。 当您想要停止侦听器时，您可以运行**lsnrctl 停止**。

### 虚拟机的防火墙中打开端口 1521

1.  仍在登录到您的虚拟机，在 Windows 中，单击**开始**，键入**具有高级安全性的 Windows 防火墙**，然后单击**具有高级安全性的 Windows 防火墙**图标。 这将打开**具有高级安全性的 Windows 防火墙**管理控制台。

2.  在防火墙管理控制台中，在左窗格中 （如果您看不到**入站规则**，展开左窗格中的顶级节点），单击**入站规则**，然后在右窗格中单击**新建规则**。

3.  **规则类型**，选择**端口**，然后单击**下一步**。

4.  有关**的协议和端口**，选择**TCP**，选择**特定的本地端口**、 **1521年**输入端口，，然后单击**下一步**。

5.  选择**允许该连接**，然后单击**下一步**。

6.  接受默认设置的配置文件规则的适用，然后单击**下一步**。

7.  为规则和说明 （可选） 指定一个名称，然后单击**完成**。

### 创建公共的 1521 终结点

1.  登录到[Azure 的门户](https://ms.portal.azure.com/)。

2.  单击**浏览**。

3.  单击**虚拟机**。

4.  选择虚拟机。

5.  单击**设置**。

6.  单击**终结点**。

7.  单击**添加**。

8.  指定终结点的名称︰

    一。 使用**TCP**协议。

    b。 使用公用端口**1521年**。

    c。 使用专用端口**1521年**。

9.  将其余选项保留-是。

10. 单击**确定**。

##启用 Oracle 数据库企业管理器的远程访问
如果您想要启用远程访问到 Oracle 数据库企业管理器中，在您的防火墙中打开端口 5500 并为 5500 Azure 门户 （使用前面打开 1521 和为 1521年创建终结点所示的步骤） 中创建一个虚拟机的终结点。 然后，若要从远程计算机运行 Oracle 企业管理器中，打开一个浏览器 url 形式的`http://<<unique_domain_name>>:5500/em`。

> [AZURE.NOTE] 可以确定的值*\<\<唯一\_域\_名称\>\>* [Azure 门户](https://ms.portal.azure.com/)通过单击**虚拟机**，然后选择您用来运行 Oracle 数据库的虚拟机中)。

##配置常用选项和高级的选项的包
如果您选择了**Oracle 数据库常用的选项**或**使用高级选项包的 Oracle 数据库**下, 一步是配置插件功能在您的 Oracle 安装。 请参考 Oracle 文档说明有关在 Windows 上，设置这些配置千差万别广泛根据您需要的每个单独的组件。

**与常用的选项包的 Oracle 数据库**包括 Oracle 数据库企业版和许可证包含的[分区](http://www.oracle.com/us/products/database/options/partitioning/overview/index.html)、[活动数据保护](http://www.oracle.com/us/products/database/options/active-data-guard/overview/index.html)、 [Oracle 数据库的优化包](http://docs.oracle.com/html/A86647_01/tun_ovw.htm)、 [Oracle 数据库的诊断包](http://docs.oracle.com/cd/B28359_01/license.111/b28287/options.htm#CIHIHDDJ)和[Oracle 数据库的生命周期管理包](http://www.oracle.com/technetwork/oem/lifecycle-mgmt-495331.html)的实例。

**使用高级选项包的 Oracle 数据库**中常用的选项包，再加上[高级压缩](http://www.oracle.com/us/products/database/options/advanced-compression/overview/index.html)[高级安全性](http://www.oracle.com/us/products/database/options/advanced-security/overview/index.html)、[标签](http://www.oracle.com/us/products/database/options/label-security/overview/index.html)、[数据库存储库](http://www.oracle.com/us/products/database/options/database-vault/overview/index.html)、[高级分析](http://www.oracle.com/us/products/database/options/advanced-analytics/overview/index.html)、 [OLAP](http://docs.oracle.com/cd/E11882_01/license.112/e47877/options.htm#CIHGDEEF)、[空间和关系图](http://docs.oracle.com/cd/E11882_01/license.112/e47877/options.htm#CIHGDEEF)、[内存中的数据库缓存](http://www.oracle.com/technetwork/products/timesten/overview/timesten-imdb-cache-101293.html)、[数据屏蔽包](http://docs.oracle.com/cd/E11882_01/license.112/e47877/options.htm#CHDGEEBB)和 Oracle 测试数据管理包 （作为数据屏蔽包的一部分） 包含许可证包含的所有选项的实例。

##其他资源
现在，您已经设置了您的虚拟机并创建您的数据库，请参见下列主题的其他信息。

-   [Oracle 虚拟机映像的其他考虑事项](virtual-machines-miscellaneous-considerations-oracle-virtual-machine-images.md)

-   [Oracle 数据库 12 c 文档库](http://www.oracle.com/pls/db1211/homepage)

-   [从 Java 应用程序连接到 Oracle 数据库](http://docs.oracle.com/cd/E11882_01/appdev.112/e12137/getconn.htm#TDPJD136)

-   [Oracle 的 Azure 的虚拟机映像](virtual-machines-oracle-list-oracle-virtual-machine-images.md)

-   [Oracle 数据库 2 天 DBA 12 c 版本 1](http://docs.oracle.com/cd/E16655_01/server.121/e17643/toc.htm)
