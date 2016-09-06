---
ms.openlocfilehash: d2116d450832481da6e9b6c5080c1fd4f63133b2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="CDN 的通过压缩文件来提高性能" 
    description="可以提高文件传输的速度，并且提高页面加载通过压缩文件的性能。" 
    services="cdn" 
    documentationCenter=".NET" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cdn" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="juliako"/>

#通过压缩文件来提高性能

本主题讨论如何提高文件传输的速度，并通过压缩文件来提高页面加载性能。

有 CDN 可以支持压缩的两种方法︰ 

- 您可以在源服务器，此时 CDN 将默认支持压缩和压缩的文件送交客户端上启用压缩。 
- 您可以启用 CDN 边缘服务器，将压缩的文件和向最终用户提供该案例 CDN 上直接压缩。

##定义

- **接受编码**的请求标头的此标头指示用户代理支持的压缩方法。 它必须包含在请求标头。 有关详细信息，请参阅[标头字段定义](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)。
- 需要为压缩添加**内容类型**的内容类型的列表。 只有文字文件可以进行压缩。 例如 纯文本、 文本/html。
- **压缩方法**-支持 gzip/陀螺/bzip2 压缩方法，必须接受编码的请求的标头中设置受支持的方法。 
- 所请求的内容已经缓存在靠近申请者的弹出窗口上标识**缓存状态**的缓存状态。  
- **文件大小**-默认压缩仅支持 1 个字节大于和小于 1 MB 的文件。  

##工作流

1. 请求者发送一个请求的内容。
2. 边缘服务器会检查是否存在**接受编码**标头。
    1. 如果包括，此标头标识请求的压缩方法。
    1. 如果缺少这种类型的请求将在非压缩格式提供服务。
3.  最近的边缘 POP 检查缓存状态、 压缩方法，如果它仍具有有效的 TTL。
    1.  缓存未命中︰ 如果不缓存请求的版本，则将该请求转发到原点。
    2.  缓存命中与相同的压缩方法︰ 边缘服务器会立即向客户端提供压缩的内容。
    3.  缓存命中与不同的压缩方法︰ 边缘服务器将编码转换资产对该请求的压缩方法。 
    4.  高速缓存命中和未压缩︰ 如果初始请求导致资产要缓存未压缩格式，则将执行检查以确定该请求是否适合边缘服务器压缩 （基于上面的定义中的要求一节中的标准）。
        1.  如果符合条件，边缘服务器将压缩该文件，并为它向客户端提供服务。
        2.  如果不符合条件︰ 边缘服务器会立即向客户端提供的未压缩的内容。 

![文件压缩](./media/cdn-file-compression/cdn-compress-files.png)

##注意事项 

1. 媒体服务 CDN 启用流终结点时，默认情况下，为以下内容类型启用压缩︰ application/vnd.ms-sstr+xml,application/dash+xml,application/vnd.apple.mpegurl,application/f4m+xml。 您不能启用/禁用压缩，从而提供所述类型使用 Azure 的门户。  
2. 将边缘服务器上缓存只能有一个文件版本 （压缩或解压缩）。 不同版本的请求将导致正在由边缘服务器转换代码的内容。  
测试
