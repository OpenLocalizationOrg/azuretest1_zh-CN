---
ms.openlocfilehash: c1f651838d8d731f496db50395db0a875388277f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 高级模板操作的资源管理器"
   description="描述如何将应用程序部署到 Azure 时使用 Azure 资源管理器模板中的嵌套的模板和复制操作。"
   services="na"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="na"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="AzurePortal"
   ms.workload="na"
   ms.date="06/29/2015"
   ms.author="tomfitz"/>

# 高级的模板的操作

本主题介绍的复制操作和嵌套的模板可以用于在将资源部署到 Azure 时执行更高级的任务。

## 复制

使您能够循环访问指定数量的时间部署资源时。
   
因为您可以循环访问数组中的每个元素的数组使用复制操作就显得特别有用。 **CopyIndex()**函数返回迭代的当前值。 您可以部署三个名为的 web 站点︰

- examplecopy Contoso
- examplecopy Fabrikam
- examplecopy 天地

使用以下模板。

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      },
      "count": { 
         "type": "int", 
         "defaultValue": 3 
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
             "count": "[parameters('count')]" 
          }, 
          "properties": {} 
      } 
    ]

您还可以使用复制操作而无需一个数组。 例如，您可能需要向已部署的每个资源名称的末尾添加一个递增的数字。 您可以将部署三个名为的 web 站点︰

- examplecopy 0
- examplecopy-1
- examplecopy-2。

使用以下模板。

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

您会注意到在前面的示例中的索引值是从 0 到 2。 偏移的索引值，可以在**copyIndex()**函数，如**copyIndex(1)**中传递值。 要执行的迭代次数仍指定在复制元素中，但 copyIndex 的值减去指定的值。 因此，与前面的示例中，使用相同的模板，但指定**copyIndex(1)**将部署三个名为的 web 站点︰

- examplecopy-1
- examplecopy-2
- examplecopy-3

## 嵌套的模板

您可能需要合并两个模板，或启动一个父级的子模板。 您可以通过使用指向该嵌套模板的主模板内部署资源来实现此目的。 您设置**templateLink**属性到嵌套模板的 URI。 您可以提供参数值的嵌套模板通过指定直接在您的模板中的值，或通过链接到参数文件。 第一个示例使用**参数**属性直接指定参数值。

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "nestedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/ArmTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

下一个示例使用**parametersLink**属性链接到的参数文件。

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "nestedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/ArmTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/ArmTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

## 下一步行动
- [Azure 的资源管理器模板创作](./resource-group-authoring-templates.md)
- [Azure 的资源管理器模板函数](./resource-group-template-functions.md)
- [将与 Azure 资源管理器模板的应用程序部署](azure-portal/resource-group-template-deploy.md)
- [Azure 的资源管理器概述](./resource-group-overview.md)

测试
