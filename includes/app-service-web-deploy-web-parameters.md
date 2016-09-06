---
ms.openlocfilehash: dc00fc1d1bc2dee0fc0de88dcd6b6cb3d613b51c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
使用 Azure 资源管理器中，您可以定义您希望部署模板时指定的值的参数。 该模板包含名参数，其中包含所有参数值的部分。
您应定义这些值，将基于您正在部署的项目或根据要部署到的环境而变化的参数。 未定义的值始终保持相同的参数。 每个参数值用于在模板中定义部署的资源。 

在定义参数时，使用**allowedValues**字段指定哪些用户可以在部署过程中提供的值。 使用**默认值**字段将值赋给该参数，如果未提供值在部署过程中。

我们将介绍每个模板中的参数。

### 站点名

您想要创建的 web 应用程序的名称。

    "siteName":{
      "type":"string"
    }

### hostingPlanName

应用程序服务计划用于承载该 web 应用程序的名称。
    
    "hostingPlanName":{
      "type":"string"
    }

### siteLocation

要用于创建 web 应用程序和托管计划的位置。 它必须是 Azure 支持 web 应用程序的位置之一。

    "siteLocation":{
      "type":"string"
    }

### sku

托管计划定价层。

    "sku":{
      "type":"string",
      "allowedValues":[
        "Free",
        "Shared",
        "Basic",
        "Standard"
      ],
      "defaultValue":"Free"
    }

模板定义允许 （自由、 共享、 基本或标准），此参数的值，并指派默认值 （免费），如果未不指定任何值。

### workerSize

实例大小 （小、 中或大） 承载的计划。

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
模板定义允许 （0、 1 或 2），此参数的值，并指派默认值 (0)，如果未不指定任何值。 值与小型，中型和大型。
