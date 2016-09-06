---
ms.openlocfilehash: 4899ad9294f08178e2fb3231f8c20e3327a43bec
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="测试的云服务性能 |Microsoft Azure"
   description="云服务使用 Visual Studio 的探查器的性能测试"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="patshea123"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/14/2015"
   ms.author="patshea" />


# 云服务的性能测试 

##概述

您可以通过以下方式测试云服务的性能︰

- 要收集信息的请求和连接，并检查站点统计数据，显示该服务如何执行从客户的角度，则使用 Azure 诊断。 若要开始使用，请参阅[Azure 云服务和虚拟机的配置诊断]( http://go.microsoft.com/fwlink/p/?LinkId=623009)。

- 使用 Visual Studio 的探查器获得服务的运行方式的计算方面的深入分析。 如本主题中所述，您可以使用探查器服务运行在 Azure 时测量性能。 有关如何使用事件探查器来测量性能，如计算仿真程序在本地运行的服务的信息，请参阅[测试性能的 Azure 云服务本地计算仿真程序使用 Visual Studio 的探查器](http://go.microsoft.com/fwlink/p/?LinkId=262845)。



## 选择一种性能测试方法

###使用 Azure 诊断收集︰###

- Web 页或服务，例如请求和连接的统计信息。

- 角色，如角色重新启动频率的统计信息。

- 总体运行角色的设置有关内存使用情况，例如垃圾回收器所需的时间或内存的百分比的信息。

###使用 Visual Studio 的探查器︰###

- 确定哪些功能需要最大时间。

- 度量值的计算密集型操作程序的每个部分采用了多长时间。

- 比较两个版本的服务的详细的性能报告。

- 分析内存分配中比单个内存分配级别的更多详细信息。

- 分析并发多线程代码中的问题。

使用探查器时，您可以收集数据，云服务运行在本地或在 Azure 时。

###分析本地收集数据为︰###

- 测试的云服务，例如特定工作人员的角色，不需要真实的模拟的负载的执行部件的性能。

- 以隔离方式，在受控条件下测试云服务的性能。

- 测试云服务的性能，然后将其部署到 Azure。

- 私下，测试云服务的性能，而不会影响现有的部署。

- 而不会导致费用在 Azure 中运行的测试服务的性能。

###收集到的 Azure 中的分析数据︰###

- 云服务在模拟或真实负载下的性能进行测试。

- 使用检测方法收集的分析数据，如本主题稍后介绍。

- 同一个服务在生产环境中的运行时环境中测试性能的服务。

通常模拟负载测试云服务正常下的或压力状况。

## 分析在 Azure 的云服务

发布 Visual Studio 从云服务时，可以配置文件服务，并指定性能分析设置，为您提供所需的信息。 为每个角色实例启动性能分析会话。 有关如何发布您的服务从 Visual Studio 的详细信息，请参阅[从 Visual Studio Azure 云服务发布](https://msdn.microsoft.com/library/azure/ee460772.aspx)。

若要了解有关性能分析在 Visual Studio 中的详细信息，请参阅[性能分析的初学者指南](https://msdn.microsoft.com/library/azure/ms182372.aspx)，并[通过使用分析工具分析应用程序性能](https://msdn.microsoft.com/library/azure/z9z62c29.aspx)。

>[AZURE.NOTE] 您可以启用 IntelliTrace 或分析时将发布您的云服务。 不能同时启用。

###探查器收集方法

您可以使用不同的集合方法进行分析，根据您的性能问题︰

- **CPU 采样**-此方法收集应用程序统计信息，可用于 CPU 利用率问题的初步分析。 CPU 采样是启动大多数性能调查的建议的方法。 在分析时收集 CPU 采样数据对应用程序没有影响很小。

- **检测**-此方法收集详细的计时数据，可进行重点分析，并且分析输入/输出性能问题。 检测方法记录每个条目、 退出和函数调用的函数在模块中分析运行期间。 此方法可用于收集有关代码节的详细的计时信息和了解输入和输出操作对应用程序性能的影响。 此方法被禁用运行 32 位操作系统的计算机。 仅在运行时在云服务 Azure，不是本地计算仿真程序中，此选项才可用。

- **.NET 内存分配**-此方法通过使用采样分析方法收集.NET Framework 的内存分配数据。 所收集的数据包括的数目和分配对象的大小。

- **并发性**-此方法收集资源争用数据，以及可用于分析多线程和多进程应用程序的进程和线程执行数据。 并发方法收集每个事件数据块执行代码，例如线程等待锁定访问应用程序资源被释放。 此方法可用于分析多线程应用程序。

- 您还可以启用**层交互分析**，它提供有关执行时间的其他信息，同步 ADO.NET 的调用中函数的多层应用程序与一个或多个数据库通信。 可以使用分析方法收集层交互数据。 层交互分析的详细信息，请参阅[层交互视图](https://msdn.microsoft.com/library/azure/dd557764.aspx)。

## 配置性能分析设置

下图演示如何配置性能分析设置，从发布 Azure 应用程序对话框。

![配置性能分析设置](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

>[AZURE.NOTE] 若要启用**启用分析**复选框，您必须使用探查器用来发布您的云服务的本地计算机上安装。 默认情况下，探查器时，安装在安装 Visual Studio。

### 若要配置性能分析设置

1. 在解决方案资源管理器中打开您 Azure 项目的快捷菜单，然后选择**发布**。 有关如何发布云服务的详细步骤，请参阅[发布云服务使用 Azure 的工具](http://go.microsoft.com/fwlink/p?LinkId=623012)。

1. 在**发布 Azure 应用程序**对话框中，选择**高级设置**选项卡。

1. 要启用分析，请选中**启用分析**复选框。

1. 配置性能分析设置，选择**设置**超链接。 在分析设置对话框出现。

1. 从**要使用哪种方法的性能分析**选项按钮中选择性能分析所需的类型。

1. 若要收集层交互分析数据，请选择**启用层交互分析**复选框。

1. 要保存设置，请选择**确定**按钮。

    在发布该应用程序时，这些设置用于创建为每个角色在性能分析会话。

## 查看分析报告

在云服务中的角色的每个实例创建一个性能分析会话。 若要查看从 Visual Studio 的每个会话的分析报表，可以查看服务器资源管理器窗口，然后选择选择角色的实例的 Azure 计算节点。 下面的插图所示，然后可以查看分析报告。

![查看从 Azure 的分析报告](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### 若要查看分析报告

1. 若要在 Visual Studio 中查看服务器资源管理器窗口，请在菜单栏上选择视图服务器资源管理器。

1. 选择 Azure 计算节点，然后选择所选配置文件从 Visual Studio 发布时的云服务 Azure 部署节点。

1. 要查看实例的分析报告，在服务中选择的角色，打开快捷菜单的特定实例，，然后选择**查看分析报告**。

    从 Azure，现在下载报表，.vsp 文件，并下载的状态出现在 Azure 活动日志。 当下载完成时，选项卡中命名的 Visual Studio 编辑器显示分析报告<Role name> _<Instance Number>_ <identifier>.vsp。 显示报表的汇总数据。

1. 若要显示不同视图的报表，请在当前视图列表中，选择所需的视图类型。 有关详细信息，请参阅[分析工具报告视图](https://msdn.microsoft.com/library/azure/bb385755.aspx)。

## 下一步行动

[调试的云服务](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[从 Visual Studio 发布到 Azure 的云服务](https://msdn.microsoft.com/library/azure/ee460772.aspx)


测试
