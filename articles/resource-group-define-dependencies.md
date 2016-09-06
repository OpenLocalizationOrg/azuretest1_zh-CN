---
ms.openlocfilehash: 3fe5f6f387446ecb2d3c812e53f7461e7202bdcc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在 Azure 资源管理器模板中定义依赖项"
   description="描述如何在部署过程中设置为依赖于其他资源的一个资源。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="mmercuri"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/15/2015"
   ms.author="mmercuri"/>

# 在 Azure 资源管理器模板中定义依赖项

对于给定的资源，可以有多个上游、 子依赖项的拓扑结构的成功至关重要。 其他资源使用的资源**取决于**和**资源**属性，您可以定义依赖关系。 也可以使用**引用**功能指定依赖项。

    {
        "name": "<name-of-the-resource>",
        "type": "<resource-provider-namespace/resource-type-name>",
        "apiVersion": "<supported-api-version-of-resource>",
        "location": "<location-of-resource>",
        "tags": { <name-value-pairs-for-resource-tagging> },
        "dependsOn": [ <array-of-related-resource-names> ],
        "properties": { <settings-for-the-resource> },
        "resources": { <dependent-resources> }
    }

 还有资源链接可以定义资源和支持跨资源组定义这些关系之间的关系。

## 取决于

对于给定的虚拟机，您可能取决于已成功设置的数据库资源。 在其他情况下，您可能取决于多个节点的群集部署虚拟机使用群集管理工具之前安装中。

在您的模板取决于属性提供定义此资源依存关系的能力。 它的值可以是资源名称的逗号分隔列表。 在计算资源之间的依赖关系和其依存顺序部署资源。 当资源不是相互依存的时它们都试图并行部署。 

虽然您可能倾向使用取决于要映射您的资源之间的依赖关系，务必要了解为什么您做它因为它可能会影响您部署的性能。 例如，如果因为想要记录互连资源如何执行此操作，取决于不是正确的做法。 取决于的生命周期仅用于部署并不是可用的后期部署。 一次部署没有方法来查询这些依赖项。 通过使用取决于您运行的影响性能的风险可能会无意中分散部署引擎使用并行度，它可能会有否则。 记录并提供查询 capabililty 资源之间的关系上，应改为使用[资源链接](resource-group-link-resources.md)。

如果引用函数用来获取资源的表示，因为引用对象表示的资源依赖项，则不需要此元素。 事实上，如果没有选项，可使用的引用与取决于，指南是使用引用功能，并具有隐式引用。 这里的基本原理同样是性能。  引用定义已知是必需的因为在该模板中引用它们的隐式相关性。 通过它们的存在，它们是相关的避免了再次优化性能，并以避免分散注意力从会不必要地避免并行部署引擎的潜在风险。

## 资源

资源属性允许您指定与所定义的资源的子资源。 子资源只能定义 5 级。 值得注意的是隐式的依赖项时不创建父资源的子资源之间。 如果您需要部署后父资源的子资源，必须明确声明与取决于属性的依赖项。 

## 引用函数

引用功能允许从其他 JSON 的名称和值对或运行时资源获得其值的表达式。 引用表达式隐式地声明一个资源依赖另一个。 由**propertyPath**下面的属性是可选的如果没有指定，则引用为资源。

    reference('resourceName').propertyPath

您可以使用此元素或取决于元素指定依赖项，但不是需要使用两个相同的相关资源。 本指南是使用隐式引用以避免无意中遇到停止部署引擎的操作的并行部署方面的不必要取决于元素的风险。

若要了解详细信息，请参阅[参考函数](../resource-group-template-functions/#reference)。

## 下一步行动

- 若要了解有关创建 Azure 资源管理器模板，请参阅[创作模板](resource-group-authoring-templates.md)。 
- 有关模板中的可用函数的列表，请参见[模板函数](resource-group-template-functions.md)。


测试
