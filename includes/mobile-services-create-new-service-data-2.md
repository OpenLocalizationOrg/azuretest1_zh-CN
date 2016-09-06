---
ms.openlocfilehash: c36a5dcb6fed2af9114ea80bf75bfeea570a4743
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
若要能够将应用程序数据存储在新的移动服务，必须首先在相关联的 SQL 数据库实例中创建新表。

1. 在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。

2. 单击**数据**选项卡，然后单击**+ 创建**。

    这将显示对话框中**创建新表**。

3. 在**表名称**键入_TodoItem_，然后单击检查按钮。

  这将创建一个新存储表**TodoItem**具有默认的权限集。 这意味着应用程序键与您的应用程序一起分发，任何人都可以访问并更改表中的数据。 

    >[AZURE.NOTE] 在移动服务快速入门中使用相同的表名称。 但是，特定于给定的移动服务的架构中创建的每个表。 这是为了避免数据冲突时多个移动服务使用相同的数据库。

4. 单击新的**TodoItem**表，请检查不有任何数据行。

5. 单击**列**选项卡。 验证下列默认值自动为您创建︰ 
    
    <table border="1" cellpadding="10">
    <tr>
    <th>列名称</th>
    <th>类型</th>
    <th>索引</th>
    </tr>
    <tr>
    <td>id</td>
    <td>string</td>
    <td>Indexed</td>
    </tr>
    <tr>
    <td>__createdAt</td>
    <td>date</td>
    <td>Indexed</td>
    </tr>
    <tr>
    <td>__updatedAt</td>
    <td>date</td>
    <td><font color="transparent">-</font></td>
    </tr>
    <tr>
    <td>__version</td>
    <td>时间戳 (MSSQL)</td>
    <td><font color="transparent">-</font></td>
    </tr>   
    </table>    
        

    这是一个表中移动服务的最低要求。 

    > [AZURE.NOTE] 在您的移动服务启用动态架构时，新列将 JSON 对象发送到移动服务的插入或更新操作时自动创建。

现在您可以将新的移动服务用作数据存储应用程序。
