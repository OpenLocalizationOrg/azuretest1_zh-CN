---
ms.openlocfilehash: 28b14413aa448d4fac9f130ef09bf0eeb9389f16
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="应用程序框架"
   description="描述如何通过使用模板使用 Azure 资源管理器中创建常用的应用程序框架。 示例包括灯泡堆栈、 SharePoint 以及 SQL Server。"
   services="virtual-machines"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="infrastructure"
   ms.date="07/02/2015"
   ms.author="rasquill"/>

# 通过使用模板创建的应用程序框架

使用此类东西快速创建善。

| Template | 说明 | 查看模板 | 立即对其进行部署 |
|:---|:---|:---:|:---:|
| 快速部署*n* Vm | 这是 Microsoft 创建的模板部署了*n*新 Vm （以及一个新虚拟网络和存储的帐户）。 | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/resource-loop-vms-vnet-aset/) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fresource-loop-vms-vnet-aset%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| 活动目录林和域 | 此模板将部署两个新的虚拟机 （以及一个新的虚拟网络、 存储帐户和负载平衡器），并创建一个新的活动目录林和域。 每个 VM 创建为新域 DC 并放在可用性组。 每个 VM 还必须具有一个公共的负载平衡的 IP 地址添加一个 RDP 终结点。 | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Factive-directory-new-domain-ha-2-dc%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| Apache web 服务器 | 此模板使用 Azure Linux CustomScript 扩展部署到 Apache web 服务器。 该模板创建 Ubuntu VM，安装 Apache2，并创建一个简单的 HTML 文件。| [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/apache2-on-ubuntu-vm) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fapache2-on-ubuntu-vm%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>
| Couchbase 群集 | 此模板部署 Couchbase 群集 Ubuntu 的虚拟机上。 | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/couchbase-on-ubuntu) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fcouchbase-on-ubuntu%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| DataStax 群集 | 此模板在 Ubuntu 的虚拟机上安装 DataStax 群集使用 Azure Linux CustomScript 扩展名。 [详细的演练。](virtual-machines-datastax-template.md)| [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/datastax-on-ubuntu) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fdatastax-on-ubuntu%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| DataStax 企业群集 | 此模板在 Ubuntu 的虚拟机上安装 DataStax 企业群集使用 Azure Linux CustomScript 扩展名。 [详细的演练。](virtual-machines-datastax-enterprise-template.md)| [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/datastax-enterprise) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fdatastax-enterprise%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| Django 应用程序 | 此模板部署在 Ubuntu VM 的 Django 应用程序通过使用 Azure Linux CustomScript 扩展。 那样 Python 和 Apache 静默式的安装。 | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/django-app) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fdjango-app%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| Docker 容器 | 此模板允许您部署 Docker （通过使用 Docker 扩展名） 和三个 Docker 容器直接来自 Docker 集线器和 Docker 撰写通过部署 Ubuntu VM。 | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fdocker-simple-on-ubuntu%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| Elasticsearch 群集 | 此模板部署在 Ubuntu Vm 上的 Elasticsearch 群集和使用模板链接来创建数据节点比例单位。 | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Felasticsearch%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| Hortonworks HDP | 此模板创建多服务器的 Hortonworks HDP 2.2 Apache Hadoop 部署 CentOS 的虚拟机，并且可以在群集内配置 HDP 安装。|  [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/hortonworks-on-centos) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fhortonworks-on-centos%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>|
| Jenkins 主设备和从设备节点 | 此模板部署在 Ubuntu VM Jenkins 主节点和两个额外的虚拟机上的多个 Jenkins 从属节点。 | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/jenkins-on-ubuntu) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fjenkins-on-ubuntu%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| Kafka 群集 | 此模板通过使用 Azure Linux CustomScript 扩展部署 Kafka 群集在 Ubuntu 的虚拟机上。 该模板还会创建一个可公开访问的虚拟机作为"jumpbox"，因此您可以诊断或诊断目的的 Kafka 节点到 SSH。| [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/kafka-on-ubuntu) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%kafka-on-ubuntu%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| 在 Ubuntu 上灯堆栈 | 此模板使用 Azure Linux CustomScript 扩展来创建执行静默式安装 MySQL，Apache，和 PHP，Ubuntu VM，然后创建一个简单的 PHP 脚本部署一个灯的应用程序。 | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Flamp-app%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| MongoDB | 此模板部署 MongoDB Ubuntu 虚拟机上使用 Linux CustomScript 扩展名。 [详细的演练。](virtual-machines-mongodb-template.md)| [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-ubuntu) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fmongodb-on-ubuntu%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| Redis 群集 | 此模板部署 Redis 群集 Ubuntu 的虚拟机上。 该模板还会创建一个可公开访问的虚拟机作为"jumpbox"，因此您可以诊断或诊断目的的 Redis 节点到 SSH。 [详细的演练。](virtual-machines-redis-template.md)| [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/redis-high-availability) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fredis-high-availability%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| SharePoint 服务器场 | 此模板将创建三个新 Azure 的虚拟机，每个都有一个公用 IP 地址、 负载平衡器和一个虚拟的网络。 它将配置一个虚拟机可用于新林和域，一个与 SQL Server 域加入，第三个 SharePoint 场和站点 Active Directory 的 DC。 所有虚拟机都有面向公众的 RDP。 [详细的演练。](virtual-machines-rmtemplate-sharepoint-walkthrough.md) | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/sharepoint-three-vm) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsharepoint-three-vm%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| 触发群集 | 此模板部署触发群集 Ubuntu 的虚拟机上。 该模板还会创建一个可公开访问的虚拟机作为"jumpbox"，因此您可以诊断或诊断目的的触发节点到 SSH。 [详细的演练。](virtual-machines-spark-template.md)| [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/spark-ubuntu-multidisks) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fspark-ubuntu-multidisks%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| Tomcat 和 OpenJDK 安装 | 此模板安装 Ubuntu VM 在 OpenJDK 和 Tomcat。 | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/openjdk-tomcat-ubuntu-vm) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fopenjdk-tomcat-ubuntu-vm%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| WordPress | 此模板将完整的灯泡堆栈部署到一个单独的 Ubuntu VM 安装和初始化 WordPress。 | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/wordpress-single-vm-ubuntu) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fwordpress-single-vm-ubuntu%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |
| ZooKeeper 群集 | 该模板在 Ubuntu 的虚拟机上创建三节点 ZooKeeper 群集。 | [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/zookeeper-cluster-ubuntu-vm) | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fzookeeper-cluster-ubuntu-vm%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> |

## 下一步行动

发现在[GitHub](https://github.com/Azure/azure-quickstart-templates)上供您使用的所有模板。

了解有关[Azure 资源管理器](../resource-group-template-deploy.md)的详细信息。
