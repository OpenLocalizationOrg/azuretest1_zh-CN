---
ms.openlocfilehash: e58a8e90a5d3d3091c293f80f11d684119190bd1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


1. 在管理门户中，单击**数据**选项卡，然后单击**登记表**。 

    ![](./media/mobile-services-update-registrations-script/mobile-portal-data-tables-registrations.png)

2. 在**登记**中，单击**脚本**选项卡并选择**插入**。
   
    ![](./media/mobile-services-update-registrations-script/mobile-insert-script-registrations.png)

    这将显示在**登记表**中插入时将调用该函数。

3. 插入函数替换为下面的代码，然后单击**保存**:

        function insert(item, user, request) {
            var registrationTable = tables.getTable('Registrations');
            registrationTable
                .where({ handle: item.handle })
                .read({ success: insertChannelIfNotFound });
            function insertChannelIfNotFound(existingRegistrations) {
                if (existingRegistrations.length > 0) {
                    request.respond(200, existingRegistrations[0]);
                } else {
                    request.execute();
                }
            }
        }

   这将注册新插入脚本，它将注册信息存储在新表中。

