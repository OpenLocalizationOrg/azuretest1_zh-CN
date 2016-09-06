---
ms.openlocfilehash: 7e748e8079296c7ffcd182574edda173b5c8ed98
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

默认情况下，可以以匿名方式调用中移动应用程序端的 Api。 接下来，您需要限制对只有已通过身份验证的客户端的访问。  

1. 在您的 PC 在 Visual Studio 中打开服务器项目并定位到**控制器** > **TodoItemController.cs**。

2. 添加`[Authorize]` **TodoItemController**类中，特性，如下所示。 这就需要由经过身份验证的用户进行的所有操作对 TodoItem 表。 要限制只为特定方法的访问，还可以将此特性应用不仅仅与那些方法，而不是类。


        [Authorize]
        public class TodoItemController : TableController<TodoItem>
   
    这就需要由经过身份验证的用户进行的所有操作对 TodoItem 表。 要限制只为特定方法的访问，还可以将此特性应用不仅仅与那些方法，而不是类。
   
3. 重新发布服务器项目。
