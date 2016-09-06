---
ms.openlocfilehash: 69694602a4c65543d0d96468e6dc14041627dadc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
### 应用程序服务计划

创建用于承载该 web 应用程序的服务计划。 提供计划通过**hostingPlanName**参数的名称。 该计划的位置是用于 web 应用程序相同的位置。 **Sku**和**workerSize**参数中指定了定价层和工作人员大小

    {
       "apiVersion":"2015-04-01",
       "name":"[parameters('hostingPlanName')]",
       "type":"Microsoft.Web/serverfarms",
       "location":"[parameters('siteLocation')]",
       "properties":{
         "name":"[parameters('hostingPlanName')]",
         "sku":"[parameters('sku')]",
         "workerSize":"[parameters('workerSize')]",
         "numberOfWorkers":1
       }
    }
