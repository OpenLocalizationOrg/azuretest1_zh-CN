---
ms.openlocfilehash: 624d163a19ee366cb4abae7e9bb0a92b63384d77
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


1. 登录到 Azure 管理门户，单击**移动服务**，然后选择移动服务。

2. 单击**API**选项卡，然后单击**创建**。 这将显示对话框中**创建新的自定义 API** 。

3. 在**API 名称**，键入_completeall_ ，然后单击检查按钮来创建新的 API。

    > [AZURE.NOTE] 使用默认的权限，与应用程序的键的任何人都可以调用自定义的 API。 但是，应用程序键因为它可能不会分发或安全地存储不会认为安全的凭据。 考虑到只有已通过身份验证用户对于附加的安全性限制访问。

4. 单击**completeall** API 表中。

5. 单击**脚本**选项卡，现有代码替换为以下代码，然后单击**保存**。    此代码使用[mssql 对象]来访问**todoitem**表直接来设置`complete`在所有项目上的标志。 由于使用**exports.post**函数，则客户端将发送 POST 请求来执行该操作。 已更改的行的数目为整数值返回给客户端。


        exports.post = function(request, response) {
            var mssql = request.service.mssql;
            var sql = "UPDATE todoitem SET complete = 1 " +
                "WHERE complete = 0; SELECT @@ROWCOUNT as count";
            mssql.query(sql, {
                success: function(results) {
                    if(results.length == 1)
                        response.send(200, results[0]);
                }
            })
        };



> [AZURE.NOTE]
> <a href="http://msdn.microsoft.com/library/windowsazure/jj554218.aspx" target="_blank">请求</a>和<a href="http://msdn.microsoft.com/library/windowsazure/dn303373.aspx" target="_blank">响应</a>对象提供给自定义的 API 函数实现的<a href="http://go.microsoft.com/fwlink/p/?LinkId=309046" target="_blank">Express.js 库</a>。 有关详细信息，请参阅<a href="http://msdn.microsoft.com/library/windowsazure/dn280974.aspx" target="_blank">自定义 API</a>。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[mssql 对象]: http://msdn.microsoft.com/library/windowsazure/jj554212.aspx
