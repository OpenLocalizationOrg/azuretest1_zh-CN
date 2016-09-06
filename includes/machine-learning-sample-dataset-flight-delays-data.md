---
ms.openlocfilehash: 7806f168b685bf944429e4905fa467c2c5a673ff
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
乘客飞行上时性能数据取自美国运输部的 TranStats 数据集合 (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">准时</a>)。<p> </p>数据集包括 4 月-10 月 2013年的时间段。 之前上传到 Azure ML Studio，数据集是按下面这样处理︰<ul><li>已筛选数据集涵盖只在美国大陆 70 繁忙机场</li><li>已取消的航班已标记如延迟 15 分钟以上</li><li>转向的航班都被筛选掉</li><li>选择以下各列︰ 年、 月、 DayofMonth、 星期、 承运人、 OriginAirportID、 DestAirportID、 CRSDepTime、 DepDelay、 DepDel15、 CRSArrTime、 ArrDelay、 ArrDel15、 已取消</li></ul>