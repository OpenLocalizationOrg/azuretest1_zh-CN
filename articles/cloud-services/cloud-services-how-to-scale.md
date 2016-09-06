---
ms.openlocfilehash: a2e7c44eb8a62db13138701be903680dc3462f8a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何扩展云服务 |Microsoft Azure" 
    description="了解如何调整在 Azure 的云服务和链接的资源。" 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/16/2015"
    ms.author="adegeo"/>





# 如何扩展应用程序

在 Azure 管理门户的小数位数页上，您可以手动扩展您的应用程序或您可以设置参数，则自动缩放。 您可以扩展 Web 角色、 工作人员角色或虚拟机运行的应用程序。 若要缩放的应用程序正在运行的 Web 角色或辅助角色实例，您添加或删除角色实例，以满足工作负载。

当向上扩展应用程序或关闭正在运行的虚拟机，新机将不创建或删除，但是打开还是关闭从以前创建的计算机的可用性。 您可以指定缩放根据平均 CPU 使用率的百分比或基于队列中的消息数。

配置您的应用程序扩展之前，应考虑以下信息︰

- 您创建的虚拟机必须添加设置来调整使用它们的应用程序可用性。 添加虚拟机可以最初打开或关闭功能，但将不断增长的行动开启和关闭向下伸缩的行动。 有关虚拟机和可用性设置的详细信息，请参阅[管理虚拟机的可用性](../virtual-machines-manage-availability.md)。

- 缩放会影响核心用法。 更大的角色实例或虚拟机上采用多个内核。 对于您的订购仅可扩充核心的限制中的应用程序。 例如，如果您的预订已限制为 20 个内核和运行的应用程序两个中等大小的虚拟机 （共四个核心），您可以只是扩大其他云服务部署在您的订阅中的十六个内核。 在可用性集中在扩展应用程序中使用的所有虚拟机都必须具有相同的大小。 有关核心用法和机大小的详细信息，请参阅[虚拟机和 Azure 的云服务规模](http://msdn.microsoft.com/library/dn197896.aspx)。

- 您必须创建一个队列，并将其与角色或设置之前，您可以扩展消息阈值所基于的应用程序的可用性。 有关详细信息，请参阅[如何使用队列存储服务](../storage-dotnet-how-to-use-queues.md)。

- 您可以扩展链接到云服务的资源。 有关链接的资源的详细信息，请参阅[方法︰ 将资源链接到云服务](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service)。

- 若要使您的应用程序的高可用性，您应该确保它使用两个或多个角色实例或虚拟机部署。 有关详细信息，请参阅[服务级别协议要求](http://azure.microsoft.com/support/legal/sla/)。


## 手动调整运行 Web 角色或辅助角色的应用程序

在比例上，可以手动增加或减少的云服务中正在运行的实例数。

1. 在[管理门户网站](https://manage.windowsazure.com/)中，单击**云服务**，然后单击要打开的仪表板的云服务的名称。

2. 单击**小数位数**。 默认情况下为所有角色，这意味着您可以手动更改应用程序使用的实例数，则禁用自动缩放。

    ![缩放页面][manual_scale]

3. 在云服务中的每个角色都有一个滑块来更改要使用的实例的数量。 要添加的角色实例，请拖动栏右侧。 要删除一个实例，请拖动左侧的栏。
    
    ![小数位数角色][slider_role] 
    
    您只能增加适当数量的核心可以用来支持实例时所使用的实例数。 滑块的颜色表示您的订阅中使用并且可用内核︰
    
    - 蓝色代表由所选角色的内核
    
    - 深灰色表示核心订阅中使用的所有角色和虚拟机
    
    - 浅灰色表示可用于缩放的内核
    
    - 粉色代表所做的更改尚未保存

4. 单击**保存**。 要添加或删除基于您的选择角色实例。

## 自动缩放 Web 角色、 工作人员角色或虚拟机运行的应用程序

在比例上，可以配置您的云服务来自动增大或减小的实例或应用程序使用的虚拟机数量。 您可以配置扩展基于下列参数︰

- [平均 CPU 使用率](#averagecpu)-如果 CPU 使用率的平均百分比超过或低于指定的阈值，角色实例中创建或删除，或者虚拟机将是打开还是关闭从可用性设置。
- [队列消息](#queuemessages)-如果队列中的消息数超过或低于指定的阈值，角色实例中创建或删除，或者虚拟机是开启或关闭从可用性设置。

## 平均 CPU 使用率

1. 在[管理门户网站](https://manage.windowsazure.com/)中，单击**云服务**，然后单击要打开的仪表板的云服务的名称。

2. 单击**小数位数**。

3. 滚动到角色或可用性集的部分，然后单击**CPU**。 这使得自动按比例缩放应用程序根据其使用的 CPU 资源的平均百分比。

    ![在自动缩放比例][autoscale_on]

4. 每个角色或可用性集都有一个滑块，用于更改可用的实例数。 若要设置可使用的最大次数，请向右右拖动栏。 要设置的最小可用的实例数，请在左侧向左拖动栏。
    
    **注意︰**在小数位数页**实例**表示角色实例或虚拟机器的实例。
    
    ![实例范围][instance_range]
    
    在订阅中可用的内核受限制的最大实例数。 滑块的颜色表示您的订阅中使用并且可用内核︰
    
    - 蓝色代表角色可用的内核的最大数量。
    
    - 深灰色表示核心订阅中使用的所有角色和虚拟机。 此值与重叠的角色使用的内核，颜色变成深蓝色。
    
    - 浅灰色表示可用于缩放的内核。
    
    - 粉色代表进行更改未保存。

5. 一个滑块用于指定的 CPU 使用率平均百分比的范围。 当 CPU 使用率的平均百分比高于最大设置，创建更多角色实例或虚拟机处于打开状态。 当 CPU 使用率的平均百分比低于最小值的设置时，删除角色实例或虚拟机已关闭。 设置最大平均 CPU 百分比，请在右侧向右拖动栏。 要设置的最小平均 CPU 百分比，请在左侧向左拖动栏。

    ![目标 cpu][target_cpu]

6. 您可以指定实例添加或打开您的应用程序将按比例放大每次的数量。 为了提高创建或打开您的应用程序将按比例放大的实例数，请拖动条权利。 若要减少数量，请拖动将条形向左。

    ![向上扩展 cpu][scale_cpuup]

7. 设置上次缩放操作和下一步大规模向上扩展的操作之间等待的分钟数。 上一次的缩放操作可以向上扩展或向下伸缩。

    ![正常运行时间][scale_uptime]

    所有的实例都包括在计算的平均 CPU 使用率的百分比时，平均值基于使用上的前一个小时。 根据您的应用程序正在使用的实例的数量，花费的时间比指定的等待时间缩放操作，如果等待时间设置得非常低。 缩放操作之间的最短时间为 5 分钟。 缩放操作情况下无法进行的任何实例都处于转换状态。

8. 您还可以指定删除或关闭您的应用程序按比例缩小的实例数。  为了提高被删除或关闭应用程序时按比例缩小的实例数，请拖动栏右侧。 若要减少数量，请拖动将条形向左。

    ![向下缩放 cpu][scale_cpudown]
    
    如果您的应用程序可以在 CPU 使用率有突然增加，您必须确保您有足够的实例来处理它们的最小数量。

9. 设置上次缩放操作和下一步的规模下操作之间等待的分钟数。 上一次的缩放操作可以向上扩展或向下伸缩。

    ![停机时间][scale_downtime]

10. 单击**保存**。 缩放操作可能需要 5 分钟才能完成。

## 队列消息

1. 在[管理门户网站](https://manage.windowsazure.com/)中，单击**云服务**，然后单击要打开的仪表板的云服务的名称。
2. 单击**小数位数**。
3. 滚动到角色或可用性集的部分，然后单击**队列**。 这使得自动按比例缩放的应用程序基础的目标队列的邮件数。

    ![小数位数队列][scale_queue]

4. 每个角色或可用性在云服务中设置有一个滑块，用于更改可用的实例数。 若要设置可使用的最大次数，请向右右拖动栏。 要设置的最小可用的实例数，请在左侧向左拖动栏。

    ![队列的范围][queue_range]
    
    **注意︰**在小数位数页**实例**表示角色实例或虚拟机器的实例。
    
    在订阅中可用的内核受限制的最大实例数。 滑块的颜色表示您的订阅中使用并且可用内核︰
    - 蓝色代表角色可用的内核的最大数量。
    - 深灰色表示核心订阅中使用的所有角色和虚拟机。 此值与重叠的角色使用的内核，颜色变成深蓝色。
    - 浅灰色表示可用于缩放的内核。
    - 粉色代表进行更改未保存。

5. 选择与您要使用的队列相关联的存储帐户。

    ![存储名称][storage_name]   

6. 选择队列。

    ![队列名称][queue_name]

7. 指定您希望支持的每个实例的消息数。 将缩放实例基础除以目标数的每台计算机的邮件的邮件总数。

    ![消息号][message_number]

8. 您可以指定实例添加或打开您的应用程序将按比例放大每次的数量。 要增加添加或打开您的应用程序将按比例放大的实例数，请拖动栏右侧。 若要减少数量，请拖动将条形向左。

    ![向上扩展 cpu][scale_cpuup]

9. 设置上次缩放操作和下一步大规模向上扩展的操作之间等待的分钟数。 上一次的缩放操作可以向上扩展或向下伸缩。

    ![正常运行时间][scale_uptime]
    
    缩放操作之间的最短时间为 5 分钟。 缩放操作情况下无法进行的任何实例都处于转换状态。

10. 您还可以指定删除或缩小您的应用程序时，使用的次数。  使用滑块来指定缩放增量。 若要增加的次数被删除或未使用向下缩放应用程序时，请拖动栏右侧。 若要减少数量，请拖动将条形向左。

    ![向下缩放 cpu][scale_cpudown]

11. 设置上次缩放操作和下一步的规模下操作之间等待的分钟数。 上一次的缩放操作可以向上扩展或向下伸缩。

    ![停机时间][scale_downtime]

12. 单击**保存**。 缩放操作可能需要 5 分钟才能完成。

## 扩展链接的资源

通常当您扩展角色，最好扩展应用程序还使用该数据库。 如果您链接到云服务的数据库，更改 SQL 数据库版并调整大小比例页面上的数据库。

1. 在[管理门户网站](https://manage.windowsazure.com/)中，单击**云服务**，然后单击要打开的仪表板的云服务的名称。
2. 单击**小数位数**。
3. 在链接的资源部分中，选择要使用的数据库的版本。

    ![链接的资源][linked_resources]

4. 选择数据库的大小。
5. 单击**保存**以更新链接的资源。

## 安排您的应用程序的缩放比例

您可以安排自动按比例缩放的应用程序通过不同时间配置的时间表。 以下选项可供您的自动缩放︰

- **没有计划**-这是默认选项，并使应用程序能够自动缩放相同的方式在所有的时间。

- **天和 night** -此选项使您能够指定天和 night 缩放的特定时间。

**注意︰**计划目前没有可用的应用程序，使用虚拟机。

1. 在[管理门户网站](https://manage.windowsazure.com/)中，单击**云服务**，然后单击要打开的仪表板的云服务的名称。
2. 单击**小数位数**。
3. 在小数位数页中，单击**设置计划时间**。

    ![缩放日程表][scale_schedule]

4. 选择缩放您想设置的日程安排类型。

5. 指定一天开始时间和结束并设置时区。 天和 night 安排，时间表示夜晚的剩余时间表示的开始和结束的一天。

6. 单击底部的页后，可以保存日程复选标记。

7. 保存计划后，它们将显示在列表中。 您可以选择您想要使用，然后修改刻度设置的时间计划。 缩放设置将仅应用在您选择的时间表。 您可以通过单击**设置计划时间**编辑日程安排。

[manual_scale]: ./media/cloud-services-how-to-scale/CloudServices_ManualScaleRoles.png
[slider_role]: ./media/cloud-services-how-to-scale/CloudServices_SliderRole.png
[autoscale_on]: ./media/cloud-services-how-to-scale/CloudServices_AutoscaleOn.png
[instance_range]: ./media/cloud-services-how-to-scale/CloudServices_InstanceRange.png
[target_cpu]: ./media/cloud-services-how-to-scale/CloudServices_TargetCPURange.png
[scale_cpuup]: ./media/cloud-services-how-to-scale/CloudServices_ScaleUpBy.png
[scale_uptime]: ./media/cloud-services-how-to-scale/CloudServices_ScaleUpWaitTime.png
[scale_cpudown]: ./media/cloud-services-how-to-scale/CloudServices_ScaleDownBy.png
[scale_downtime]: ./media/cloud-services-how-to-scale/CloudServices_ScaleDownWaitTime.png
[scale_queue]: ./media/cloud-services-how-to-scale/CloudServices_QueueScale.png
[queue_range]: ./media/cloud-services-how-to-scale/CloudServices_QueueRange.png
[storage_name]: ./media/cloud-services-how-to-scale/CloudServices_StorageAccountName.png
[queue_name]: ./media/cloud-services-how-to-scale/CloudServices_QueueName.png
[message_number]: ./media/cloud-services-how-to-scale/CloudServices_TargetMessageNumber.png
[linked_resources]: ./media/cloud-services-how-to-scale/CloudServices_ScaleLinkedResources.png
[scale_schedule]: ./media/cloud-services-how-to-scale/CloudServices_SetUpSchedule.png
 

测试
