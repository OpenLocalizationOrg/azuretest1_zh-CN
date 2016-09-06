---
ms.openlocfilehash: f6754d76a22971843bd2bd8ba27b4d71c81b1587
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
数据派生自维基百科 (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) 根据每个 S 和 P 500 家公司，作为 XML 数据存储中的项目。<p> </p>之前上传到 Azure ML Studio，数据集是按下面这样处理︰<ul><li>对于每个特定公司提取文本内容</li><li>删除 wiki 格式</li><li>删除非字母数字字符</li><li>将所有文本都转换为小写</li><li>添加已知的公司类别</li></ul><p> </p>无法找到请注意，对于某些公司的文章，因此记录数小于 500。