---
ms.openlocfilehash: 0f2683d7304bbbbf5d9647eda91baa5a3abf9658
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="认可的 Azure 中的 Linux 分发" 
    description="了解 Linux 在 Azure 认可的分配，包括 Ubuntu，OpenLogic，和 SUSE 的指导原则。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/03/2015" 
    ms.author="szark"/>



#Linux 在 Azure 认可分配

Azure 库中的 Linux 图像由很多的伙伴，而且我们正在与各种 Linux 社区认可的通讯组列表中添加更多风格。 同时，库中不可用的分布通常可以携带您的自己的 Linux 通过在[此页](virtual-machines-linux-create-upload-vhd.md)上遵循以下指南。


## 受支持的版本和版本 ##

下表列出的 Linux 发行版本和在 Azure 受支持的版本。

Hyper-V 和 Azure 的 Linux 集成服务 (LIS) 驱动程序 Microsoft 提供直接到上游 Linux 内核的内核模块。  LIS 驱动程序默认情况下，或者内置于分发的内核或为旧 RHEL CentOS 基于/分发并可单独下载[这里](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)。  请[这篇文章](virtual-machines-linux-create-upload-vhd-generic.md#linux-kernel-requirements)有关的 LIS 驱动程序的详细信息，参阅。

Azure Linux 代理已预安装在 Azure 库图像和程序通常可从分发软件包存储库。  在[GitHub](https://github.com/azure/walinuxagent)上可以找到源代码。

通讯组|版本|驱动程序|代理
---|---|---|---
规范的 Ubuntu|Ubuntu 12.04，14.04、 14.10 和 15.04|在内核中|在下"walinuxagent"repo 包︰ <p><p>[GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)的来源︰
CentOS 的 OpenLogic |CentOS 6.3 + 7.0 +| CentOS 6.3︰ [LIS 下载](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>在内核中 centOS 6.4 +:|包︰ 在<a href="http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/">OpenLogic repo 下"WALinuxAgent"<p><p>[GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)的来源︰
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/)|494.4.0+ |在内核中|[GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)的来源︰
Oracle Linux| 6.4 +、 7.0 +|在内核中|在下"WALinuxAgent"repo 包︰<p><p>[GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)的来源︰
SUSE Linux 企业 |SLES 11 SP3 以上、 SLES 12 + 和  <p><p> 针对 SAP 11.3 + SLES |在内核中|在"WALinuxAgent"在[云︰ 工具](https://build.opensuse.org/project/show/Cloud:Tools)repo 包︰<p><p>[GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)的来源︰
openSUSE |openSUSE 13.1 +|在内核中|在"WALinuxAgent"在[云︰ 工具](https://build.opensuse.org/project/show/Cloud:Tools)repo 包︰ <p><p>[GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)的源代码︰

## 合作伙伴

### 规范
[http://www.ubuntu.com/cloud/azure](http://www.ubuntu.com/cloud/azure)

规范的工程，在客户端、 服务器和云计算，包括个人云服务的使用者开放社区治理驱动器 Ubuntu 成功。 规范的 ubuntu，通过电话为云用一系列的电话、 触、 电视和桌面，连贯接口统一免费平台的设想使 Ubuntu 多样化机构公共云提供商提供的消费性电子产品制造商和单个的技术专家们收藏的第一选择。

与开发人员和世界各地的工程中心，Canonical 具有得天独厚的优势合作硬件制造商、 内容提供商和软件开发人员将 Ubuntu 解决方案推向市场，从电脑到服务器和手持设备。


### CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

从 CoreOS 网站︰

*CoreOS 专为安全性、 一致性和可靠性。 而不是安装软件包通过 yum 或比较简练，CoreOS 使用 Linux 容器来管理您的服务级别较高的抽象。 单个服务的代码和所有依赖项打包在可以运行在一个或多个 CoreOS 机器的容器内。*


### OpenLogic
[http://www.openlogic.com/azure](http://www.openlogic.com/azure)

OpenLogic 是云和数据中心的企业开放源码解决方案的一流提供商。 OpenLogic 帮助数百家主要跨范围广泛的行业，若要安全地获得、 支持和控制开放源代码软件的企业。 OpenLogic 提供了商业级技术支持和赔偿 600 开源软件包 OpenLogic 的专家社区，包括 CentOS 的企业级支持，以及在 Azure 上提供基于 CentOS 映像正在启动合作伙伴作为后盾。


### Oracle
[http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle 的策略是同时为客户提供选择和灵活性它们如何部署 Oracle 软件 Oracle 群以及其他云提供广泛的公共云和私有云的解决方案。  Oracle 的与 Microsoft 的合作关系使客户能够部署 Oracle 软件在 Microsoft 公共云和私有云认证满怀信心和从 Oracle 支持。  Oracle 的承诺以及在 Oracle 公有和私有云解决方案上的投资不会改变。


### SUSE
[http://www.suse.com/suse-linux-enterprise-server-on-azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

在 SUSE Linux 企业服务器在 Azure 上是一个成熟的平台，提供优异的可靠性和安全性的云计算。 SUSE 的通用 Linux 平台无缝集成与 Azure 的云服务提供一个易于管理的云环境。 和多 1800 SUSE Linux 企业服务器的独立软件供应商的多个 9,200 认证应用程序，使用 SUSE 来确保在数据中心运行受支持的工作负载可以在 Azure 上信心十足地部署。

 
