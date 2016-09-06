---
ms.openlocfilehash: 4269cec45128258495236b1c9a702ed10eb60c5b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


最后，您必须更新注册到要发送通知的 TodoItem 表上执行插入操作的脚本。

1. 单击**TodoItem**，单击**脚本**，然后选择**插入**。 

2. 插入函数替换为下面的代码，然后单击**保存**:

        function insert(item, user, request) {
        // Define a payload for the Windows Store toast notification.
        var payload = '<?xml version="1.0" encoding="utf-8"?><toast><visual>' +    
            '<binding template="ToastText01">  <text id="1">' +
            item.text + '</text></binding></visual></toast>';
        
        request.execute({
            success: function() {
                // If the insert succeeds, send a notification.
                push.wns.send(null, payload, 'wns/toast', {
                    success: function(pushResponse) {
                        console.log("Sent push:", pushResponse);
                        request.respond();
                        },              
                        error: function (pushResponse) {
                            console.log("Error Sending push:", pushResponse);
                            request.respond(500, { error: pushResponse });
                            }
                        });
                    }
                });
        }

    此插入脚本插入成功后，对所有 Windows 应用商店应用程序登记发送推式通知 （带有插入的项的文本）。

