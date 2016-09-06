---
ms.openlocfilehash: 649a38d535e625c4f947493a4bc47f2e80f94f83
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
在这一节中添加两个新用户到您的目录以及新的销售组。 一个用户将授予成员资格的销售组。 其他用户将不授予成员资格的组。 

### 创建用户


1. 在 [Azure 管理门户] 导航到以前配置的身份验证完成本教程向您的应用程序中添加身份验证的目录。
2. 单击顶部的页上的**用户**。 然后单击底部的**添加用户**按钮。 
3. 完成创建名为**Bob**的用户而创建的新用户对话。 注意用户的临时密码。 
4. 创建名为**Dave**的另一个用户。 注意用户的临时密码。
5. 新用户应类似于下面显示的内容。

    ![](./media/mobile-services-aad-rbac-create-sales-group/users.png)    


### 创建销售组


1. 在目录页中，单击页面顶部的**组**。 然后单击底部的**添加组**按钮。 
2. 输入组的名称**销售**，并按创建组对话框上的完成按钮。 

    ![](./media/mobile-services-aad-rbac-create-sales-group/sales-group.png)

### 添加到销售组的用户成员身份。


1. 单击目录页顶部的**组**。 然后单击要转到销售组页的**销售**组。 
2. 在销售组页中，单击**添加成员**。 添加名为销售组到**Bob**的用户。 名为**Dave**的用户不应组的成员。

    ![](./media/mobile-services-aad-rbac-create-sales-group/group-membership.png)

3. 在销售组页上单击**属性**，然后复制**对象 ID**在页面底部的销售组。 

   
    ![](./media/mobile-services-aad-rbac-create-sales-group/sales-group-id.png)

4. 向后定位到您的移动服务的配置页面，并添加为应用程序设置已命名的对象 id **AAD\_销售\_组\_ID**。 本教程使用而不是查找基于组名称的 id 的应用程序设置为组的对象 id。 这是因为组名称可能会更改在其中 id 保持不变。

    ![](./media/mobile-services-aad-rbac-create-sales-group/sales-group-id-app-setting.png)