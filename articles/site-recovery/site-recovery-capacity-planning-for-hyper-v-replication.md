---
ms.openlocfilehash: 9e01b0513c3cb465f8da27bd827204b34383f1ac
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="容量规划 Hyper-V 复制"
    description="这篇文章包含说明如何使用 Azure 站点恢复，容量计划工具，包括用于容量规划 Hyper-V 复制资源"
    services="site-recovery"
    documentationCenter="na"
    authors="csilauraa"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="08/05/2015"
    ms.author="lauraa" />

# 容量规划 Hyper-V 复制

Azure 站点恢复使用 Hyper-V 副本或内部部署版本的 VMM 服务器和 Azure 存储两个内部 VMM 站点之间的复制。 本文包含有关使用容量规划师的 Azure 站点故障恢复 (ASR)-Hyper-V 副本工具的分步指导。 ASR 的容量规划师工具 Hyper-V 副本指导 IT 管理员在设计服务器、 存储和网络基础结构所需，若要成功部署 Hyper-V 副本并验证两个站点之间的网络连接。

## 系统要求
- 操作系统︰ Windows Server® 2012年或 Windows Server® 2012 R2
- 内存︰ 20 MB （最小）
- CPU︰ 开销 5%（最少）
- 磁盘空间︰ 5 MB （最少）

## 教程的步骤
- 第 1 步︰ 准备主站点
- 步骤 2︰ 准备恢复站点恢复站点是否内部
- 步骤 3︰ 运行容量计划工具
- 步骤 4︰ 解释结果

## 第 1 步︰ 准备主站点
1. 制作需要复制和相应主 Hyper-V 主机/群集启用 Hyper-V 虚拟机的所有列表。
2. 主要 Hyper-V 主机和群集分组为下列情况之一︰

  - Windows Server® 2012年独立服务器
  - Windows Server® 2012年群集
  - Windows Server® 2012 R2 独立服务器
  - Windows Server® 2012 R2 群集

3. 您将需要运行独立服务器组每一次，一次为每个群集的容量计划工具。
4. 启用远程访问 WMI 上所有主要主机和群集。 确保设置了适当的防火墙规则和用户权限集。

        netsh firewall set service RemoteAdmin enable

5. 启用性能监视主要主机上。

  - 使用**高级安全**管理单元，打开**Windows 防火墙**，然后启用下列入站的规则︰
    - COM + 网络访问 (DCOM IN)
    - 远程事件日志管理组中的所有规则

## 步骤 2︰ 准备恢复站点
如果使用 Azure 作为恢复站点，或尚未准备好内部恢复站点，然后可以跳过此部分。 但是，如果您跳过它，您将无法测量两个站点之间的可用带宽。

1. 标识身份验证方法

    一。 Kerberos︰ 使用时的主和恢复 Hyper-V 主机位于同一域中或在相互信任的域中。

    b。 证书︰ 使用主和恢复 Hyper-V 主机时在不同的域。 可以使用 makecert 来创建证书。 有关部署证书使用这种方法所需的步骤的信息，请参阅[Hyper-V 副本证书基于身份验证的 makecert](http://blogs.technet.com/b/virtualization/archive/2013/04/13/hyper-v-replica-certificate-based-authentication-makecert.aspx)博客文章。

2. 标识一个**一个**从恢复站点恢复 Hyper-V 主机/群集。

    一。 将使用此恢复主机/群集能够复制虚拟虚拟机，并估计主要和辅助站点之间的可用带宽。

    b。 **推荐**︰ 运行测试时使用的单个恢复 Hyper-V 主机。

### 准备一台 Hyper-V 主机为恢复服务器
1. 在 Hyper-V 管理器中，单击**操作**窗格中的**Hyper-V 设置**。
2. 在**Hyper-V 设置**对话框中，单击**复制配置**。
3. 在详细信息窗格中，选择**启用此计算机作为副本服务器**。
4. 在**认证和端口**部分中，选择以前选择的身份验证方法。 哪种身份验证方法，用于指定要使用的端口 （默认端口为 80 kerberos 通过 HTTP 和基于证书的身份验证的 443 通过 HTTPS）。
5. 如果您正在使用基于证书的身份验证，单击**选择证书**，然后提供证书信息。
6. 在**授权和存储**部分中，指定允许**任何**经过身份验证的 （主） 服务器向此副本服务器发送复制数据。
7. 单击**确定**或**应用**。

![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

8. 验证 https 侦听器正在执行命令︰ netsh http 显示 servicestate
9. 打开防火墙端口︰

        Port 443 (certificae-based authentication):
          Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"


        Port 80 (Kerberos):
          Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"

### 准备一个 Hyper-V 群集作为恢复目标
如果您已经准备好独立 Hyper-V 主机恢复服务器，然后您可以跳过本节。

1. 配置 Hyper-V 复制代理程序︰

    一。 在**服务器管理器**中，打开**故障转移群集管理器**。

    b。 在左窗格中，将连接到该群集，并在群集名称将突出显示，在**操作**窗格中单击**配置角色**。 这将打开**高可用性向导**。

    c。 在**选择角色**屏幕中，选择**Hyper-V 复制代理程序**。

    d。 完成向导，并提供要用作连接的**NetBIOS 名称**和**IP 地址**指向群集 （称为客户端访问点）。 配置**Hyper-V 副本代理**，从而导致客户端访问点名称。 记下客户端的访问点名称-将在以后配置副本。

    电子。 验证 Hyper-V 副本中介角色成功联机，并且可以故障转移群集的所有节点之间。 若要做到这一点，右键单击角色，指向**移动**，然后单击**选择节点**。 请选择一个节点 >**确定**。

    f。 如果您使用基于证书的身份验证，请确保每个群集节点和 Hyper-V 副本代理的客户端访问点的所有已安装的证书。

2. 配置复制设置︰

    一。 在**服务器管理器**中，打开**故障转移群集管理器**。
    
    b。 在左窗格中，将连接到该群集，并时突出显示群集名称时，单击**角色**中的**详细信息**窗格中的**导航**类别。
    
    c。 该角色中，用鼠标右键单击，然后选择**复制设置**。
    
    d。 在**详细信息**窗格中，选择**启用此作为副本服务器群集**。

    电子。 在**身份验证和端口**部分中，选择您在前面选择的身份验证方法。 哪种身份验证方法，用于指定要使用的端口 （默认端口是 80 Kerberos 通过 HTTP 和基于证书的身份验证的 443 通过 HTTPS）。

    f。 如果您正在使用基于证书的身份验证，单击**选择证书**，然后提供 reqeusted 证书信息。

    g。 在**授权和存储**部分中，指定是否允许**任何**经过身份验证的 （主） 服务器向此副本服务器发送复制数据或从特定的主服务器限制为数据的验收。 您可以使用通配符来限制接受到服务器从特定域而无需指定它们每个分别 (例如，*。 contoso.com)。

    h。 打开所有恢复 Hyper-V 主机上的防火墙端口︰ 端口 443 （证书授权机构）︰ 获取 ClusterNode |ForEach 对象 {0} 调用命令名\$\_.name 简言之 {启用 Netfirewallrule displayname"Hyper-V 副本 HTTPS 侦听器 （TCP 中）"}}


          Port 80 (Kerberos auth):
              Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}


## 步骤 3︰ 运行容量计划工具
1. 下载的容量规划师工具。
2. 从一个主服务器 （或主群集中的节点之一） 运行该工具。 该.exe 文件，用鼠标右键单击，然后选择**以管理员身份运行**。
3. 如果您选择，接受**许可证条款**，，然后单击**下一步**。
4. 选择**的度量标准收集的持续时间**。 强烈建议该工具运行在生产时间，以确保最具代表性的数据被收集。 度量标准收集的建议持续时间为 30 分钟。 如果仅要验证网络连接，您可以选择持续时间为一分钟。

![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

5. 指定**主站点的详细信息**，如所示，，然后单击**下一步**。

    对于一台独立主机，请输入服务器名或 FQDN。

    如果您的主要主机是群集的一部分，您可以输入 FQDN 下列项之一︰

    一。 Hyper-V 副本代理客户端访问点 (CAP)

    b。 群集名称

    c。 在群集的任何节点

  ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)


6. 输入**复制副本站点详细信息**（本地站点复制到本地站点）

  如果您想要启用复制到 Azure 或没有准备 Hyper-V 主机或群集作为恢复服务器 （如第 2 步中所述），则应跳过测试涉及到副本站点。

  指定**副本站点**的详细信息，并单击**下一步**。

我。 对于一台独立主机，输入服务器名或 FQDN

二。 如果您的主要主机是群集的一部分，您可以输入 FQDN 下列项之一︰

一。 Hyper-V 副本代理客户端访问点 (CAP)

b。 群集名称

c。 在群集的任何节点

   ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

7. 跳过测试涉及**扩展副本站点**。 他们不支持 ASR。
8. 选择虚拟机的配置文件。 该工具将连接到**主站点的详细信息**，在指定的群集或独立服务器，并枚举包含正在运行的虚拟机。 选择虚拟机和虚拟磁盘，您想要收集的指标。

不会枚举或显示以下虚拟机︰

- 已启用复制的虚拟机
- 未运行的虚拟机

9. 输入**网络信息**（这是只适用于内部站点到内部站点复制，提供了复制副本站点详细信息）。

指定请求的网络信息中，，然后单击**下一步**。

- 估计的 WAN 带宽
- 证书用于身份验证 （可选）︰ 如果您打算使用基于证书的身份验证，则应在此页上提供所需的证书。

   ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

10. 在接下来的屏幕，请单击**下一步**将启动该工具。

![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

11. 该工具完成运行，单击**查看报告**以查看输出。

    默认报表位置︰

    %systemdrive%\Users\Public\Documents\Capacity 计划器

    日志位置︰

    %systemdrive%\Users\Public\Documents\CapacityPlanner

## 步骤 4︰ 解释结果
是不是这种情况下相关，则可以忽略不出以下两种情况下，指标。

### 内部站点复制到本地站点
  - 主要主机计算、 记忆上的复制的影响
  - 上主，恢复主机存储的磁盘空间，IOPS 的复制的影响
  - 所需的增量复制 (Mbps) 的总带宽
  - 主要主机之间恢复主机 (Mbps) 的观察的网络带宽
  - 这两个主机群集之间的活动并行传输的理想数目的建议

### Azure 复制到本地站点
  - 主要主机计算、 记忆上的复制的影响
  - 主要主机存储的磁盘空间，IOPS 的复制的影响
  - 所需的增量复制 (Mbps) 的总带宽

[容量规划师 Hyper-V 副本下载页面](http://go.microsoft.com/?linkid=9876170)上可以找到更详细的指导。

## 其他资源
下列资源可帮助您规划 Hyper-V 复制的容量︰

- [更新︰ Hyper-V 副本容量规划师](http://go.microsoft.com/fwlink/?LinkId=510891)— — 阅读有关这种新工具的概述此网络日志项。

- [容量规划师 Hyper-V 副本](http://go.microsoft.com/fwlink/?LinkId=510892)— — 下载此工具的最新版本。

- [引导式的动手实验](http://go.microsoft.com/fwlink/?LinkId=510893)，获得很好的容量规划工具李小明 Mayer TechNet 博客上演练。

- [性能和扩展测试的内部到内部部署](https://msdn.microsoft.com/library/azure/dn760892.aspx)— — 阅读的复制内部内部部署到测试结果。


## 下一步行动

若要启动 ASR 的部署︰

- [设置内部 VMM 站点和 Azure 之间的保护](site-recovery-vmm-to-azure.md)
- [设置 Azure 上部署 Hyper-V 站点之间的保护](site-recovery-hyper-v-site-to-azure)
- [设置两个内部 VMM 站点之间的保护](site-recovery-vmm-to-vmm)
- [设置两个内部 VMM 站点使用 SAN 之间的保护](site-recovery-vmm-san)
- [设置与一个 VMM 服务器保护](site-recovery-single-vmm)
 
