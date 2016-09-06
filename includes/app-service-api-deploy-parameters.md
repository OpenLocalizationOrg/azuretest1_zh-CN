---
ms.openlocfilehash: 549af3a6156d9f83c2c5de32f92b38d711a68e73
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
使用 Azure 资源管理器中，您可以定义您希望部署模板时指定的值的参数。 该模板包含名参数，其中包含所有参数值的部分。
您应定义这些值，将基于您正在部署的项目或根据要部署到的环境而变化的参数。 未定义的值始终保持相同的参数。 每个参数值用于在模板中定义部署的资源。 

我们将介绍每个模板中的参数。

### gatewayName

网关的名称。 API 的应用程序获取注册到此网关。

    "gatewayName": {
      "type": "string"
    }

### apiAppName

API 的应用程序创建的名称。 该名称必须包含至少 8 个字符，不能超过 50 个字符。
    
    "apiAppName": {
      "type": "string"
    }

### apiAppSecret

API 的应用程序的秘密。 此值必须是使用 base64 编码的字符串。 它应该是一个随机字符串使用 64 个字符，并只有整数和小写字符组成。

    "apiAppSecret": {
      "type": "securestring"
    }

### 位置

新的 API 应用程序的位置。 您可以通过运行 PowerShell 命令获取有效位置`Get-AzureLocation`Azure CLI 命令或`azure location list`。

    "location": {
      "type": "string"
    }

