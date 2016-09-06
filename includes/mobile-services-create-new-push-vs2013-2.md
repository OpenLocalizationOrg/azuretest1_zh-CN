---
ms.openlocfilehash: 24e294afda18ac39681b19a00f38b90e7ce60a2f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 在**通道**表的 insert.js 文件，找到下面的代码行，注释掉它们或删除该文件，然后保存您的更改。

        sendNotifications(item.channelUri);

        function sendNotifications(uri) {
            console.log("Uri: ", uri);
            push.wns.sendToastText01(uri, {
                text1: "Sample toast from sample insert"
            }, {
                success: function (pushResponse) {
                    console.log("Sent push:", pushResponse);
                }
            });
        }
        
    当您将更改保存到 insert.js 文件中时，新的脚本版本上载到您的移动服务。

2. 在服务器资源管理器中展开的 TodoItem 表、 打开 insert.js 文件当前插入函数替换为以下代码，然后保存您的更改︰ 

        function insert(item, user, request) {
            request.execute({
                success: function() {
                    request.respond();
                    sendNotifications();
                }
            });
        
            function sendNotifications() {
                var channelsTable = tables.getTable('channels');
                channelsTable.read({
                    success: function(devices) {
                        devices.forEach(function(device) {
                            push.wns.sendToastText04(device.channelUri, {
                                text1: item.text
                            }, {
                                success: function(pushResponse) {
                                    console.log("Sent push:", pushResponse);
                                }
                            });
                        });
                    }
                });
            }
        }
        
    现在，当您插入新的 TodoItem，推式通知是发送到所有已注册的设备。
