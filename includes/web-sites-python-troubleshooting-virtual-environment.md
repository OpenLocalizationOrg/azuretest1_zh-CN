---
ms.openlocfilehash: 7d3d2d1e7c73cfdef3401af7b96672625786093f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
如果它检测到兼容的虚拟环境已不存在，部署脚本将在 Azure 上跳创建虚拟环境。  这可以大大加快部署。  Pip 将跳过已安装的软件包。

在某些情况下，您可能需要强制删除该虚拟环境。  您需要这样做，如果您想为您的存储库的一部分包括一个虚拟环境。  您可能还要这样做，如果您需要去除某些程序包，或 requirements.txt 对更改进行测试。

有几个选项来管理现有的虚拟环境中在 Azure 上︰

### 选项 1︰ 使用 FTP

使用 FTP 客户端，连接到服务器，您可以删除环境文件夹。  请注意某些 FTP 客户端 （如 web 浏览器） 可能是只读的并且不允许您删除文件夹，因此您需要确保 FTP 客户端使用该功能。  FTP 主机名称和用户名显示在[Azure 门户网站](https://portal.azure.com)上的 web 应用程序的刀片式服务器。

### 选项 2︰ 切换运行时

这里是事实的利用部署脚本将删除环境文件夹，当它与 Python 所需的版本不匹配的一种替代方法。  这有效地将删除现有的环境中，并创建一个新。

1. 切换到另一版本的 Python （通过 runtime.txt 或在 Azure 门户**应用程序设置**刀片式服务器）
1. git 推一些更改 （如果有的话，则忽略任何 pip 安装错误）
1. 切换回的 Python 的初始版本
1. git 再次推一些更改

### 选项 3︰ 自定义部署脚本

如果您已自定义部署脚本，您可以更改 deploy.cmd 强迫它删除环境文件夹中的代码。