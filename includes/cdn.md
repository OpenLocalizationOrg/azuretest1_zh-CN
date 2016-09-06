---
ms.openlocfilehash: 2d18ef00c6a6e0885d65836192879d3c8686a118
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
# Azure 使用 CDN

Azure 内容传递网络 (CDN) 为开发人员提供提供高带宽内容缓存 blob 和静态内容的计算实例，在美国、 欧洲、 亚洲、 澳大利亚和南美洲中的物理节点的全球解决方案。 CDN 节点位置的当前列表，请参阅[Azure CDN 节点位置]。

此任务包括以下步骤︰

* [步骤 1︰ 创建存储帐户。](#Step1)
* [步骤 2︰ 创建新存储帐户的 CDN 终结点](#Step2)
* [第 3 步︰ 访问您 CDN 的内容](#Step3)
* [步骤 4︰ 删除您 CDN 的内容](#Step4)

使用 CDN 缓存 Azure 数据的优点包括︰

-   更高的性能和更好的用户体验有很的内容源，并使用应用程序需要多个互联网行程加载内容的最终用户
-   分布式的规模大，更好地处理瞬时高负载，说，开头的事件，如产品上市

CDN 的现有客户现在可以在[Azure 管理门户]中使用 Azure CDN。 CDN 是附加功能向您的订购，有单独[付费的计划]。

<a id="Step1"> </a>
<h2>步骤 1︰ 创建存储帐户。</h2>

使用以下过程来创建新的存储帐户 Azure 订阅。 存储帐户有权访问 Azure 存储服务。 存储帐户代表访问了 Azure 存储服务组件的每个命名空间的最高级别︰ 服务、 队列服务和表服务的 Blob。 Azure 存储服务有关的详细信息，请参阅[使用 Azure 存储服务](http://msdn.microsoft.com/library/azure/gg433040.aspx)。

若要创建存储帐户，您必须是服务管理员或相关联的订阅的联管理员。

> [AZURE.NOTE] 有关通过使用 Azure 服务管理 API 来执行此操作的信息，请参阅[创建存储帐户](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx)参考主题。

**若要创建订阅了 Azure 存储帐户**

1.  登录到[Azure 的管理门户]。
2.  在左下角，单击**新建**。 在**新建**对话框中，选择**数据服务**，然后单击**存储**，然后**快速创建**。

    此时将显示**创建存储帐户**对话框。

    ![创建存储帐户][create-new-storage-account]

4. 在**URL**字段中，键入一个子域的名称。 此项可包含 3-24 个小写字母和数字。

    此值将成为用于订阅的 Blob、 队列或表资源的地址的 URI 中的主机名称。 为解决斑点服务中的容器资源，URI 中使用下面的格式，其中*&lt;StorageAccountLabel&gt;*指的是您在**输入 URL**中键入的值︰

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **重要︰**URL 标签窗体存储帐户 URI 子域和 Azure 中的所有托管服务中必须唯一。

    此值还用作在门户中，或此存储帐户的名称以编程方式访问该帐户时。

5.  从**区域中的相似性组**下拉列表，选择某个地区或存储帐户的关联组。 如果您想要在同一个数据中心与您正在使用其他 Windows Azure 服务您存储服务，请选择而不是某个地区关系组。 这样可以提高性能，并且没有费用时要交纳的出口。  

    **注意︰**要创建关系组，打开**设置**区域的管理门户，单击**好友小组**，，然后单击**添加关联组**或**添加**。 您还可以创建和管理关联组使用 Windows Azure 服务管理 API。 有关详细信息，请参阅 [好友小组的操作]。

6. 从**订阅**下拉列表中，选择将用于存储帐户订阅。
7.  单击**创建存储帐户**。 创建存储帐户的过程可能需要几分钟才能完成。
8.  若要验证已成功创建存储帐户，请验证，该帐户将出现在状态为**联机****存储**时所列的项目。

<a id="Step2"> </a>
<h2>步骤 2︰ 创建新存储帐户的 CDN 终结点</h2>

一旦您启用 CDN 访问存储帐户或承载服务，所有公开可用的对象有资格 CDN 边缘缓存。 如果您修改当前缓存在 CDN 的对象时，新内容不能通过 CDN 直到 CDN 缓存内容生存时间段到期时刷新其内容。

**若要创建新存储帐户的 CDN 终结点**

1. 在[Azure 管理门户网站]，在导航窗格中，单击**CDN**。

2. 在功能区上，单击**新建**。 在**新建**对话框中，选择**应用程序服务**，然后**CDN**，然后**快速创建**。

3. 在**源域**下拉列表中，选择列表中的可用存储帐户上一节中创建的存储帐户。 

4. 单击**创建**按钮创建新的终结点。

5. 终结点创建后，它将出现在订阅终结点的列表。 列表视图显示用于访问缓存的内容，以及原始域的 URL。 

    原始域是从中 CDN 缓存内容的位置。 原始域可以存储帐户或云服务;存储帐户用于本示例的目的。 于根据指定，缓存控件设置到或缓存的网络的默认试探法的边缘服务器缓存存储内容。 


    > [AZURE.NOTE] 创建终结点的配置将立即不可用;可能需要多达 60 分钟的登记通过 CDN 网络传播。 通过 CDN 可用内容之前，请立即使用 CDN 的域名用户可能会收到状态代码 400 （错误请求）。

<a id="Step3"> </a>
<h2>第 3 步︰ 访问 CDN 的内容</h2> 

若要访问在 CDN 缓存的内容，使用 CDN URL 提供的门户。 Blob 缓存的地址将类似如下︰

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>
<h2>步骤 4︰ 从 CDN 中删除内容</h2>

如果您不再希望缓存对象在 Azure 内容传递网络 (CDN)，您可以执行下列步骤之一︰

-   为 Azure 的 blob，可以从公共容器中删除该 blob。
-   您可以使容器而不是公钥的私钥。 有关详细信息，请参阅[限制访问容器和 Blob](http://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) 。
-   您可以禁用或删除使用管理门户的 CDN 终结点。
-   您可以修改您的托管的服务不再响应请求的对象。

对象的生存时间期限到期之前，已在 CDN 缓存的对象将保持缓存。 当生存时间段到期时，将检查 CDN CDN 终结点是否仍然有效，仍然可以以匿名方式访问该对象，请参阅。 如果不是，然后不再缓存对象。

在 Azure 管理门户上当前不支持立即清除内容的能力。 请联系[Azure 支持](http://azure.microsoft.com/support/options/)如果您需要立即清除内容。 

## 其他资源

-   [如何在 Azure 创建关系组]
-   [如何︰ 管理订阅了 Azure 存储帐户]
-   [有关 API 的服务管理]
-   [如何 CDN 的内容映射到自定义的域]

  [创建存储帐户]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
  [Azure CDN 节点位置]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
  [Azure 的管理门户]: https://manage.windowsazure.com/
  [提供商]: /pricing/calculator/?scenario=full
  [如何在 Azure 创建关系组]: http://msdn.microsoft.com/library/azure/ee460798.aspx
  [Azure 的 CDN 的概述]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
  [有关 API 的服务管理]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
  [如何 CDN 的内容映射到自定义的域]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[创建新的存储-分期付款]: ./media/cdn/CDN_CreateNewStorageAcct.png
[以前的管理门户]: ../../Shared/Media/previous-portal.png
