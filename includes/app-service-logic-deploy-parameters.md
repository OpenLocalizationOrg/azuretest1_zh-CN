---
ms.openlocfilehash: 138b3bcdfb222698f344f284ff8bdf40e397de5d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
使用 Azure 资源管理器中，您可以定义您希望部署模板时指定的值的参数。 该模板包含名参数，其中包含所有参数值的部分。
您应定义这些值，将基于您正在部署的项目或根据要部署到的环境而变化的参数。 未定义的值始终保持相同的参数。 每个参数值用于在模板中定义所部署的资源。 

在定义参数时，使用**allowedValues**字段指定哪些用户可以在部署过程中提供的值。 使用**默认值**字段将值赋给该参数，如果未提供值在部署过程中。

我们将介绍每个模板中的参数。

### logicAppName

逻辑应用程序创建的名称。

    "logicAppName": {
        "type": "string"
    }

### svcPlanName

创建用于承载逻辑应用程序的应用程序服务计划的名称。
    
    "svcPlanName": {
        "type": "string"
    }

### sku

定价的逻辑应用程序层。

    "sku": {
        "type": "string",
        "defaultValue": "Standard",
        "allowedValues": [
            "Free",
            "Basic",
            "Standard",
            "Premium"
        ]
    }

模板定义允许 （自由、 基本、 标准版或高级），此参数的值，并指派默认值 （标准），如果未不指定任何值。

### svcPlanSize

应用程序实例的大小。

    "svcPlanSize": {
        "defaultValue": "0",
        "type": "string",
        "allowedValues": [
            "0",
            "1",
            "2"
        ]
    }

模板定义允许 （0、 1 或 2），此参数的值，并指派默认值 (0)，如果未不指定任何值。 值与小型，中型和大型。
