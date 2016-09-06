---
ms.openlocfilehash: 2a6f90fc346ab1dff2aa1fffe33890875da6e525
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<table cellspacing="0" border="1">
<tr>
   <th align="left" valign="middle">资源</th>
   <th align="left" valign="middle">默认限制</th>
</tr>
<tr>
   <td valign="middle"><p>数据库大小</p></td>
   <td valign="middle"><p>取决于性能级别<sup>1</sup></p></td>
</tr>
<tr>
   <td valign="middle"><p>登录名</p></td>
   <td valign="middle"><p>取决于性能级别<sup>1</sup></p></td>
</tr>
<tr>
   <td valign="middle"><p>内存使用情况</p></td>
   <td valign="middle"><p>超过 20 秒钟 16 MB 内存授予</p></td>
</tr>
<tr>
   <td valign="middle"><p>会话</p></td>
   <td valign="middle"><p>取决于性能级别<sup>1</sup></p></td>
</tr>
<tr>
   <td valign="middle"><p>Tempdb 大小</p></td>
   <td valign="middle"><p>5 GB</p></td>
</tr>
<tr>
   <td valign="middle"><p>事务的持续时间</p></td>
   <td valign="middle"><p>24 小时<sup>2</sup></p></td>
</tr>
<tr>
   <td valign="middle"><p>每个交易记录锁定</p></td>
   <td valign="middle"><p>1 万</p></td>
</tr>
<tr>
   <td valign="middle"><p>每个交易记录的大小</p></td>
   <td valign="middle"><p>2 GB</p></td>
</tr>
<tr>
   <td valign="middle"><p>每个交易记录所用的总的日志空间的百分比</p></td>
   <td valign="middle"><p>20%</p></td>
</tr>
<tr>
   <td valign="middle"><p>最大并发请求 （辅助线程）</p></td>
   <td valign="middle"><p>取决于性能级别<sup>1</sup></p></td>
</tr>
</table>

<sup>1</sup>SQL 数据库具有性能层，如基本、 标准和津贴。 标准和最优还划分为性能级别。 每个层和服务级别的详细限制，请参阅[Azure SQL 数据库服务层和性能级别](https://msdn.microsoft.com/library/azure/dn741336.aspx)。

<sup>2</sup>如果事务锁定的基础的系统任务所需的资源，则最大持续时间为 20 秒。