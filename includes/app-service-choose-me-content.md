---
ms.openlocfilehash: 076714e75dbfe88f02d7afee52a50f3093b1d262
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<a name="tellmeas"></a>
## 告诉我有关的应用程序服务

Azure 的虚拟机可以处理广泛的云托管任务。 但是，创建和管理虚拟机基础架构需要专门的技能和大量的精力。 如果您不需要完全控制虚拟机运行您的 web 应用程序、 移动应用程序 backends、 API 的应用程序，等等，还有更容易 （并且便宜） 的解决方案︰*平台即服务*(PaaS)。 使用 PaaS，Azure 处理大部分运行您的应用程序虚拟机的管理工作。 [Azure 应用程序服务](../article/app-service/app-service-value-prop-what-is.md)是完全托管的 PaaS 解决方案，使您可以构建、 部署和扩展企业级应用程序，以秒为单位。

应用程序服务是许多类型的应用程序工作负载的最佳选择。 公司可能需要生成或迁移的商业网站时，可以处理数以百万计的点击一周并在几个数据中心中部署全球各地。 同一公司也可能有企业的活动目录，从跟踪身份验证的用户的开支报告的业务线应用程序，该应用程序可能有一个移动设备组件和连接到后端资源和业务流程。 支出报表可能需要定期在后台作业进行计算和汇总大量的信息。 IT 顾问可能采用流行的开放源码应用程序设置为小型企业内容管理系统。 下图显示了一些可以在 Azure 应用程序服务中运行的 web 应用程序的类型。

<a name="appservice_diagram"></a>
![应用程序服务图](media/app-service-choose-me-content/diagram.png)
 
**图︰ Azure 应用程序服务支持静态 web 页、 流行的 web 应用程序，并使用各种技术生成的自定义 web 应用程序。 您还可以运行移动 backends、 API 的应用程序和非 web 计算工作负载 （使用 WebJobs）。** 

使用 Azure 应用程序服务，您可以运行任何类型的计算工作负荷使用[WebJobs](../article/app-service-web/websites-webjobs-resources.md)功能。 

Azure 应用程序服务提供了在共享虚拟机包含多个应用程序创建多个用户，或仅由您使用的虚拟机上运行的选项。 虚拟机是 Azure 应用程序服务管理的资源池的一部分，从而允许获得高可靠性和容错能力。

起步非常容易。 使用 Azure 应用程序服务，用户可以从选择的应用程序、 框架和模板，并以秒为单位创建一个 web 应用程序。 他们然后可以使用他们最喜欢的开发工具 （WebMatrix，Visual Studio） 任何其他编辑器和源代码管理选项设置持续集成和开发团队。 依赖于 MySQL 数据库的应用程序能够使用 MySQL Azure 的 ClearDB，为 Microsoft 合作伙伴提供的服务。

开发人员可以使用 Azure 应用程序服务创建大型的、 可扩展的 web 应用程序。 创建使用 ASP.NET、 PHP、 Node.js 和 Python 应用程序技术支持。 例如，应用程序可以使用严格会话，并且无变化情况下，许多现有的 web 应用程序可以移动到此云平台。 在 Azure 应用程序服务构建的 web 应用程序可以使用 Azure，服务总线、 SQL 数据库和 Blob 存储等其他方面。 您还可以运行应用程序的多个副本在不同虚拟机，Azure 应用程序服务自动负载平衡请求它们之间。 因为在已经存在的虚拟机中创建新的 web 应用程序实例，启动一个新的应用程序实例发生速度非常快; 和它是比等待一个新的虚拟机创建快很多。

上[图](#appservice_diagram)所示，您可以发布代码和其他网页内容到 Azure 应用程序服务以多种方式。 您可以使用 FTP、 FTPS 或 Microsoft 的 WebDeploy 技术。 Azure 应用程序服务还支持从源代码管理系统，包括 Git，GitHub、 CodePlex、 BitBucket、 收存箱、 Mercurial，Team Foundation Server 和基于云的团队基础服务的发布代码。
