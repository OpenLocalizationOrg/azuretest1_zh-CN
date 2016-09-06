---
ms.openlocfilehash: b7a8910e99249ebbefe11177cb7afc6dca8bf0f8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="流分析限制表"
   description="介绍了系统的限制和流分析组件和连接的推荐的大小。"
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/13/2015"
   ms.author="jeffstok" />

| 限制标识符 | Limit       | Comments |
|----------------- | ------------|--------- |
| 流式处理单位的每个订阅每个地区的最大数量 | 50 | 可通过联系[Microsoft 技术支持](https://support.microsoft.com/en-us)请求，以增加流单位订购超过 50。 |
| 流式处理单位的最大吞吐量 | 1 MB / s * | 每提的最大吞吐量取决于该方案。 实际的吞吐量可能较低，取决于查询的复杂性和分区。 [小数位数 Azure 流分析作业，以增加吞吐量](../articles/stream-analytics/stream-analytics-scale-jobs.md)文章中找不到更多详细信息。 |
| SELECT 语句的查询限制 | 5 每个查询的输出 | 此限制可能会在将来增加。 |
| 子查询 SELECT 语句的限制 | 子查询每 14 聚合 | 此限制可能会在将来增加。 |
