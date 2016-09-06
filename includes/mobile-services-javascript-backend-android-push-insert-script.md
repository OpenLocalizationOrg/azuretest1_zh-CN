---
ms.openlocfilehash: 8194bf646c1b21f5dc614bd74c83b3c899de035f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 在管理门户中，单击**数据**选项卡，然后单击**TodoItem**表。 
 
2. 在**todoitem**，单击**脚本**选项卡，然后选择**插入**。
   
    这将显示插入发生**TodoItem**表中时将调用该函数。

3. 插入函数替换为下面的代码，然后单击**保存**:

        function insert(item, user, request) {
        // Define a simple payload for a GCM notification.
        var payload = {
            "data": {
                "message": item.text
            }
        };      
        request.execute({
            success: function() {
                // If the insert succeeds, send a notification.
                push.gcm.send(null, payload, {
                    success: function(pushResponse) {
                        console.log("Sent push:", pushResponse, payload);
                        request.respond();
                        },              
                    error: function (pushResponse) {
                        console.log("Error Sending push:", pushResponse);
                        request.respond(500, { error: pushResponse });
                        }
                    });
                },
            error: function(err) {
                console.log("request.execute error", err)
                request.respond();
            }
          });
        }

    这将注册一个新的插入脚本，使用[gcm 对象](http://go.microsoft.com/fwlink/p/?LinkId=282645)插入成功后向所有已注册的设备发送推式通知。 