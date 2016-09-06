---
ms.openlocfilehash: 702e2402a400a336a320091cc3d0997aba4acd9c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1.  登录到在线[Microsoft Azure 预览门户](https://portal.azure.com/)。
2.  在 Jumpbar 中，单击**新建**，然后单击**数据 + 存储**，然后单击**Azure DocumentDB**。 

    ![屏幕抓图的 Azure 预览门户中，突出显示新建按钮，数据 + 中创建刀片式服务器，存储和数据中的 Azure DocumentDB + 存储刀片式服务器][1]   

    <!-- Alternatively, from the Startboard, you can browse the Azure Marketplace, select **Data + storage**, choose **DocumentDB**, and then click **Create**.  -->
    
    <!-- ![Screen shot of the Azure preview portal, showing the Marketplace blade with the DocumentDB tile highlighted, and the DocumentDB blade with the Create database button highlighted][2]    -->
   

3. 在**新的 DocumentDB 帐户**刀片式服务器，指定所需的配置为 DocumentDB 帐户。 
 
    ![新 DocumentDB 刀片式服务器的屏幕抓图][3] 


    - 在**ID**框中，输入一个名称来标识的 DocumentDB 帐户。  **ID**进行验证后， **ID**框中将显示一个绿色复选标记。 **ID**值将变为 URI 中的主机名称。 **ID**可以包含仅小写字母、 数字和-字符，并必须在 3 到 50 个字符之间。 请注意， *documents.azure.com*被追加到您选择，其结果是将成为您的 DocumentDB 帐户终结点的终结点名称。

    - 因为 DocumentDB 支持一个单一的标准帐户层已锁定**帐户层**镜头。 有关详细信息，请参阅[DocumentDB 定价](http://go.microsoft.com/fwlink/p/?LinkID=402317&clcid=0x409)。

    - 在**资源组**中，选择或创建您的 DocumentDB 帐户的资源组。  默认情况下，将创建新的资源组。  但是，您可能，选择选择要向其中添加您的 DocumentDB 帐户的现有资源组。 有关详细信息，请参阅[使用资源组来管理 Azure 的资源](resource-group-portal.md)。

    - 对于**订阅**，选择您想要为 DocumentDB 帐户使用 Azure 订阅。 如果只有一个订阅您的帐户，该帐户是默认选中的。
 
    - 使用**位置**指定的地理位置，用来承载您的 DocumentDB 帐户。   

4.  一旦配置了新的 DocumentDB 帐户选项，请单击**创建**。  它可以需要几分钟的 DocumentDB 帐户创建。  若要检查状态，您可以监视在 Startboard 上的进度。  
    ![Startboard-联机数据库创建者创建平铺的屏幕抓图][4]  
  
    或者，您可以监控您的进度通知集线器。  

    ![通知中心，显示正在创建的 DocumentDB 帐户的屏幕抓图的-联机数据库创建者通知快速创建数据库][5]  

    ![通知中心，DocumentDB 帐户已成功创建和部署到一个资源组显示的屏幕抓图][6]

5.  创建的 DocumentDB 帐户后，就可以使用与在线门户中的默认设置。 请注意，默认一致性的 DocumentDB 帐户设置为**会话**。  您可以通过单击**DocumentDB 帐户**刀片式服务器上的**默认一致性**拼贴来调整默认一致性设置。

    ![资源组刀片的屏幕抓图][7]  

<!--Image references-->
[1]: media/documentdb-create-dbaccount/ca1.png
[2]: media/documentdb-create-dbaccount/ca2.png
[3]: media/documentdb-create-dbaccount/ca3.png
[4]: media/documentdb-create-dbaccount/ca4.png
[5]: media/documentdb-create-dbaccount/ca5.png
[6]: media/documentdb-create-dbaccount/ca6.png
[7]: media/documentdb-create-dbaccount/ca7.png

[如何︰ 创建一个 DocumentDB 帐户]: #Howto
[下一步行动]: #NextSteps
[documentdb-管理]:../articles/documentdb/documentdb-manage.md
