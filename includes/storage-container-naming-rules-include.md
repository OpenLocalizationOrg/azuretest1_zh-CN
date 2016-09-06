---
ms.openlocfilehash: d3411b5394d382f459416d4dcc5f22cae1d35324
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
在 Azure 存储中每个 blob 必须驻留在一个容器中。 Blob 名称的容器窗体部分。 例如，`mycontainer`是这些示例 blob Uri 中的容器的名称︰

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

容器名称必须是有效的 DNS 名称，符合下面的命名规则︰

1. 容器名称必须以字母或数字开头，可以包含字母、 数字和短划线 （-） 字符。
1. 每个短划线 （-） 字符必须立即前面和后面的字母或数字。容器名称中不允许连续的短划线。
1. 容器名称中的所有字母必须都为小写。
1. 容器名称必须是从 3 到 63 个字符。

> [AZURE.IMPORTANT] 请注意容器的名称必须始终为小写。 如果您在一个容器的名称，包括大小写字母或其他方式侵犯命名规则的容器，可能会收到一个 400 （错误请求） 的错误。 