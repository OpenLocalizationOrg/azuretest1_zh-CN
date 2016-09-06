---
ms.openlocfilehash: a64c0056ad69b695590bec9f7ef00082b4f66775
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="部署资源的多个实例 |Microsoft Azure"
   description="复制操作和阵列 Azure 资源管理器在模板中使用，循环多次部署资源时。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2015"
   ms.author="tomfitz"/>

# Azure 资源管理器中创建资源的多个的实例

本主题演示如何循环在 Azure 资源管理器模板来创建一个资源的多个实例。

## 复制、 copyIndex 和长度

在要创建多个时间的资源，可以定义指定的次数循环**复制**对象。 该副本采用下面的格式︰

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

您可以访问当前迭代值与**copyIndex()**函数，如下面所示的 concat 函数内。

    [concat('examplecopy-', copyIndex())]

在数组中的值创建多个资源，您可以使用**length**函数来指定计数。 您提供长度函数的参数数组。

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## 在名称中使用的索引值

您可以使用复制操作创建多个基于递增索引唯一命名的资源实例。 例如，您可能需要部署每个资源名称的末尾添加一个唯一的编号。 若要部署三个名为的 web 站点︰

- examplecopy 0
- examplecopy-1
- examplecopy-2。

应使用以下模板︰

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2014-06-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {} 
      } 
    ]

## 索引值的偏移量

您会注意到在前面的示例中的索引值是从 0 到 2。 偏移的索引值，可以在**copyIndex()**函数，如**copyIndex(1)**中传递值。 要执行的迭代次数仍指定在复制元素中，但 copyIndex 的值减去指定的值。 因此，与前面的示例中，使用相同的模板，但指定**copyIndex(1)**将部署三个名为的 web 站点︰

- examplecopy-1
- examplecopy-2
- examplecopy-3

## 与数组一起使用
   
因为您可以循环访问数组中的每个元素的数组使用复制操作就显得特别有用。 若要部署三个名为的 web 站点︰

- examplecopy Contoso
- examplecopy Fabrikam
- examplecopy 天地

应使用以下模板︰

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2014-06-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {} 
      } 
    ]

当然，您的副本数量设置为非数组的长度的值。 例如，可以创建一个数组具有多个值，并再传入参数值，它指定要部署的数组元素的多少。 在这种情况下，在第一个示例中所示设置份数。 

## 下一步行动
- 如果您想要了解模板的部分，请参阅[创作 Azure 资源管理器模板](./resource-group-authoring-templates.md)。
- 对于所有可以在模板中使用的函数，请参见[Azure 资源管理器模板函数](./resource-group-template-functions.md)。
- 若要了解如何部署您的模板，请参阅[部署了 Azure 资源管理器模板的应用程序](azure-portal/resource-group-template-deploy.md)。

测试
