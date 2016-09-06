---
ms.openlocfilehash: ea0449168363c0e561af0ca9aab301e41c192af4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 登录到 [Azure 管理门户] 上，单击**移动服务**，然后单击移动服务。

2. 单击**推送**选项卡选择**只有经过身份验证的用户**的**权限**，单击**保存**，然后单击**编辑脚本**。
    
    这允许您自定义推送通知注册回调函数。 如果您使用 Git 编辑源代码，在中找到此同一注册函数`.\service\extensions\push.js`。

3. 现有**注册**函数替换为以下代码，然后单击**保存**:

        exports.register = function (registration, registrationContext, done) {   
            // Get the ID of the logged-in user.
            var userId = registrationContext.user.userId;    
            
            // Perform a check here for any disallowed tags.
            if (!validateTags(registration))
            {
                // Return a service error when the client tries 
                // to set a user ID tag, which is not allowed.      
                done("You cannot supply a tag that is a user ID");      
            }
            else{
                // Add a new tag that is the user ID.
                registration.tags.push(userId);
                
                // Complete the callback as normal.
                done();
            }
        };
        
        function validateTags(registration){
            for(var i = 0; i < registration.tags.length; i++) { 
                console.log(registration.tags[i]);           
                if (registration.tags[i]
                .search(/facebook:|twitter:|google:|microsoft:/i) !== -1){
                    return false;
                }
                return true;
            }
        }

    这将向已登录的用户的 ID 的注册标记。 提供的标记进行验证来防止用户在注册为另一个用户 id。 当向该用户发送通知时，收到这和注册用户的任何其他设备上。

4. 单击后退箭头，单击**数据**选项卡，单击**TodoItem**、 单击**脚本**，然后选择**插入**。 