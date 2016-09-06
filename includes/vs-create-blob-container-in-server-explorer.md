---
ms.openlocfilehash: f0796d95cf1ae4cbb240b7001b121af023fec12e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
您可以通过使用**服务器资源管理器中**的 Visual Studio 创建 Azure 存储队列。

![服务器资源管理器中的 Blob][Image1]

1. 在**视图**菜单中，选择**服务器资源管理器**。
2. 在服务器资源管理器中，展开您的订购的**Azure**节点，展开**存储**节点和连接的 Azure 存储服务中指定的存储帐户的节点。
3. 选择**队列**节点，然后从上下文菜单中选择**创建队列**。
4. 输入容器的名称，然后选择**确定**。   

默认情况下，新的容器是私有的您必须指定您的存储访问密钥下载此容器中的 blob。 如果您想要公开的容器中的文件，在**服务器资源管理器**和按选择的容器`F4`以显示**属性**窗口。 对**Blob**设置**公共的读访问权限**。 在 Internet 上的任何人都可以看到 blob 在公共容器中，但您可以修改或删除它们，只有具有适当的访问键。


[Image1]: ./media/vs-create-blob-container-in-server-explorer/vs-storage-create-blob-containers-in-Server-Explorer.png