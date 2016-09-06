---
ms.openlocfilehash: 1208eb48f3418b7493673b8f76edae867e7d82fb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 什么是 Blob 存储

Azure Blob 存储是一种服务来存储大量的非结构化数据，如文本或二进制数据，可以从任何位置访问在世界中通过 HTTP 或 HTTPS。 您可以使用 Blob 存储，将数据暴露给全世界，公开或私下存储应用程序数据。

Blob 存储的常见用途包括︰

-   提供图像或直接到浏览器的文档
-   存储分布式访问的文件
-   流式传输视频和音频
-   执行安全的备份和灾难恢复
-   通过在内部存储的数据的分析或 Azure 托管服务

## Blob 服务概念

Blob 服务包含以下组件︰

![Blob1][Blob1]

-   **存储帐户︰**对 Azure 存储的所有访问都是通过存储帐户。 有关帐户的存储容量的详细信息，请参阅[Azure 存储可扩展性和性能目标](storage-scalability-targets.md)。

-   **容器︰**容器提供了一套 blob 的分组。
    所有 blob 都必须在容器中。 一个帐户可以包含任意的数量的容器。 一个容器可以存储任意的数量的 blob。

-   **Blob:**所有类型和大小的文件。 Azure 存储提供了三种类型的 blob︰ 阻止 blob、 页面 blob，并追加 blob。
    
    *块 blob*非常适合于存储文本或二进制文件，如文档和媒体文件。 *追加 blob*都类似于块 blob，组成的块，但他们则适合追加操作，因此它们可用于日志记录的方案。 一个阻止 blob 或追加 blob 可以包含多达 50000 块达 4 MB 的每个，略微超过 195 GB (4 MB X 50000) 的总大小。
    
    *页面 blob*可达 1TB 的大小，和频繁的读写操作效率更高。 Azure 的虚拟机作为操作系统和数据磁盘使用页面 blob。

    有关 blob 的详细信息，请参阅[了解块 Blob、 页面 Blob 和追加的 Blob](https://msdn.microsoft.com/library/azure/ee691964.aspx)。

## 命名和引用容器和 blob

您可以使用下面的 URL 格式存储帐户中解决 blob:
   
    http://<storage-account-name>.blob.core.windows.net/<container-name>/<blob-name>  
      
例如，下面是解决一个在上图中的 blob 的 URL:  

    http://sally.blob.core.windows.net/movies/MOV1.AVI

### 容器的命名规则

容器名称必须是有效的 DNS 名称并符合以下规则︰

- 容器名称必须为全部小写。
- 容器名称必须以字母或数字开头，可以包含字母、 数字和短划线 （-） 字符。
- 每个短划线 （-） 字符必须立即前面和后面的字母或数字。容器名称中不允许连续的短划线。
- 容器名称必须是从 3 到 63 个字符。

### 斑点的命名规则

Blob 名称必须遵循下列规则︰

- Blob 名称只能包含字符的任意组合。
- Blob 名称必须至少包含一个字符，并且不能超过 1024 个字符长。
- Blob 名称是区分大小写。
- 必须正确地转义保留的 URL 字符。
- 路径段组成的 blob 名称数不能超过 254。 路径段是连续分隔符字符之间的字符串 (*例如*，反斜杠 /) 对应于虚拟目录的名称。

Blob 服务基于一种简单的存储方案。 您可以通过指定字符或字符串分隔符中的 blob 名称来创建虚拟层次结构创建虚拟层次结构。 例如，下面的列表显示了一些有效的和唯一的 blob 名称︰

    /a
    /a.txt
    /a/b
    /a/b.txt

您可以按层次结构使用分隔符字符的列表 blob。


[Blob1]: ./media/storage-blob-concepts-include/blob1.jpg

