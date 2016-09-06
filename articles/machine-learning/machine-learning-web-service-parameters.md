---
ms.openlocfilehash: eed82f0822d615904e571552150db814e110c9d6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Azure 机器学习 Web 服务参数 |Microsoft Azure" 
    description="如何使用 Azure 机器学习 Web 服务参数来访问 web 服务时修改您的模型的行为。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2015" 
    ms.author="raymondl;garye"/>

#使用 Azure 机器学习 Web 服务参数

创建 Azure 机器学习 web 服务发布一个包含模块带有可配置参数的实验。 在某些情况下，您可能需要 web 服务正在运行时更改模块的行为。 *Web 服务参数*允许您执行此操作。 

一个常见的例子建立[读取器][读取器]模块，以便访问 web 服务时，已发布的 web 服务的用户可以指定不同的数据源。 或者配置该[编写器][写入器]模块，以便可以指定一个不同的目的地。 其他示例包括更改[功能哈希][散列功能]模块的比特数或[基于筛选器的功能选择][筛选器基于-功能-选择]模块所需的功能的数量。 

您可以定义 Web 服务参数并将其与一个或多个模块参数相关联，您可以指定它们是否必需或可选。 在访问服务时，将在运行时修改模块操作时，web 服务的用户然后可以为这些参数提供值。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##如何设置和使用 Web 服务参数

通过单击模块的参数旁边的图标，然后选择"设置为 web 服务参数"来定义 Web 服务参数。 这将创建新的 Web 服务参数并将其连接到该模块参数。 然后，当访问 web 服务时，用户可以针对 Web 服务参数指定一个值，它将应用于该模块参数。

一旦您定义了 Web 服务参数，可供实验中的任何其他模块参数。 如果您定义带有一个模块的参数关联的 Web 服务参数，可只要参数需要相同类型的值的任何其他模块中，使用该相同的 Web 服务参数。 例如，如果 Web 服务参数是一个数值，然后它只能期望数值的模块参数。 当用户设置 Web 服务参数的值时，它将应用于所有相关的模块的参数。

您可以决定是否为 Web 服务参数提供默认值。 如果那样的话，参数是可选的 web 服务的用户。 如果您没有提供默认值，然后被要求用户来访问 web 服务时输入值。

为 web 服务 （提供通过机器学习 Studio 中的**仪表板**的 web 服务的**API 帮助页**链接） 的文档将包括如何以编程方式访问 web 服务时指定 Web 服务参数的 web 服务用户信息。


##示例

例如，假定我们有一个[编写器][编写器]模块，将信息发送到 Azure blob 存储试验。 我们将定义一个名为"斑点路径"的 Web 服务参数，以允许访问该服务时，将路径更改为 blob 存储 web 服务用户。

1.  在机器学习 Studio 中，单击以将其选中该[编写器][编写器]模块。 实验画布的右侧的属性窗格中显示其属性。

2.  指定的存储类型︰

    - **请指定数据的目标位置**，请在下选择"Azure Blob 存储"。
    - **请指定身份验证类型**，请在下选择"帐户"。
    - 输入的 Azure blob 存储的帐户信息。 
    <p />

3.  单击右边的**路径到 blob 容器参数的开始位置**的图标。 它看起来像这样︰

    ![Web 服务参数图标][icon]

    选择"设置为 web 服务参数"。

    具有名称"到 blob 容器开头的路径"属性窗格底部的**Web 服务参数**下添加一个条目。 这是目前 Web 服务参数与此[编写器][写入器]模块参数相关联。

4.  要重命名 Web 服务参数，请单击名称、 输入"斑点路径"，并按**Enter**键。 
 
5.  若要为 Web 服务参数提供默认值名称右侧的图标选择"输入默认值"，输入一个值 (例如，"container1/output1.csv") 和，按**Enter**键。

    ![Web 服务参数][parameter]

6.  单击**运行**。 

7.  单击要发布 web 服务的**发布 WEB 服务**。

访问 web 服务时，web 服务的用户现在可以指定[编写器][写入器]模块的新目标。

##详细信息

更详细的示例，请参见[Web 服务参数](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx)条目中的[机器学习博客](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx)。

访问计算机学习 web 服务的详细信息，请参阅[如何使用已发布的机器学习 web 服务](machine-learning-consume-web-services.md)。



<!-- Images -->
[图标]: ./media/machine-learning-web-service-parameters/icon.png
[参数]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[哈希功能处理]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[-基于-功能-选定内容筛选]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[读取器]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[编写器]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 

测试
