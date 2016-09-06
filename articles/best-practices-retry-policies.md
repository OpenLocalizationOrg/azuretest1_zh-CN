---
ms.openlocfilehash: e75b0d4b20fc6a062afd5b2adcbb3f428c67d3d3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="NuGet 程序包 |Microsoft Azure"
   description="NuGet 程序包上一般重试策略制定工作的指南。"
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="masimms"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/09/2015"
   ms.author="masashin"/>

# NuGet 程序包

<p class="lead">随着更多的组件开始通信，瞬间失败变得更加重要，要巧妙地处理。 由重试策略 NuGet 程序包的瞬时性故障处理工作可以帮助处理单个实例中的重试次数。</p>

> 本文根据草稿为概念验证。 它不是实际的检查的指导。

模式和实践`TransientFaultHandling`代码建议用于一般重试策略工作。

```
Install-Package EnterpriseLibrary.WindowsAzure.TransientFaultHandling
```

## 配置

部分包含重试功能的配置信息︰

参数            | 说明
-------------------- | ----------------------
MaximumExecutionTime | 最长执行时间的请求，包括所有潜在的重试次数。
ServerTimeOut        | 服务器请求的超时间隔
RetryPolicy          | 重试策略。 请参阅下面的策略部分

```csharp
/// <summary>
/// An interface required for request option types.
/// </summary>
public interface IRequestOptions
{
    IRetryPolicy RetryPolicy { get; set; }

    TimeSpan? ServerTimeout { get; set; }

    TimeSpan? MaximumExecutionTime { get; set; }
}
```

以编程方式︰

- 支持的客户端上设置。
- 启用操作提供的客户端在重写

配置文件︰

```xml
<RetryPolicyConfiguration defaultRetryStrategy="Fixed Interval Retry Strategy">
    <linearInterval name="Fixed Interval Retry Strategy"
    retryInterval="00:00:01" maxRetryCount="10" />
    <exponentialBackoff name="Backoff Retry Strategy" minBackoff="00:00:01"
        maxBackoff="00:00:30" deltaBackoff="00:00:10" maxRetryCount="10"
        fastFirst="false"/>
</RetryPolicyConfiguration>
```

## 策略

### 指数

用于服务调用的反复尝试出间距呈指数级增长以避免服务限制。

__做法︰__

以指数级增加后续尝试之间的延迟时间间隔。 添加要避免所有的客户端同时重试延迟间隔 （+ 20%) 到随机

__配置︰__

参数            | 说明
-------------------- | -------------------------------------------------------
maxAttempt           | 重试次数。
deltaBackoff         | 关闭后重试间隔。 此时间跨度的倍数将用于后续重试次数。
MinBackoff           | 添加到所有重试间隔，计算出 deltaBackoff。
FastFirst            | 即时的首次重试
MaxBackoff           | 如果计算所得的重试间隔大于 MaxBackoff，则使用 MaxBackoff。 不能更改此值。

__实现逻辑︰__

```csharp
if(!ExponentialRetry.FastFirst){
    Random r = new Random();
    double increment = (Math.Pow(2, currentRetryCount) - 1) * r.Next((int)(this.deltaBackoff.TotalMilliseconds * 0.8), (int)(this.deltaBackoff.TotalMilliseconds * 1.2));
    retryInterval = (increment < 0) ? ExponentialRetry.MaxBackoff :
    TimeSpan.FromMilliseconds(Math.Min(ExponentialRetry.MaxBackoff.TotalMilliseconds, ExponentialRetry.MinBackoff.TotalMilliseconds + increment));
} else {
    retryInterval = TimeSpan.Zero;
}
```

## 线性

用于服务调用的反复尝试出间距线性以避免服务限制。

__做法︰__

执行指定的重试次数，使用指定的固定的时间间隔重试的间隔数。 添加要避免所有的客户端同时重试延迟间隔 （+ 20%) 到随机。

__配置︰__

参数            | 说明
-------------------- | -------------------------------------------------------
maxAttempt | 重试次数。
deltaBackoff | 关闭后重试间隔。
FastFirst | 即时的首次重试

__实现逻辑︰__

```csharp
if(!ExponentialRetry.FastFirst) {
    Random r = new Random();
    retryInterval = TimeSpan.FromMilliseconds(r.Next((int)(
    this.deltaBackoff.TotalMilliseconds * 0.8), (int)(this.deltaBackoff.TotalMilliseconds * 1.2)));
} else {
    retryInterval = TimeSpan.Zero;
}
```

## 自适应

用于调整间距的服务调用基于错误代码的重复尝试出 / 元数据服务的响应标头中传递。

__做法︰__

执行指定的次数，采用延迟间隔计算基于错误代码 / 元数据服务的响应标头中传递


__配置︰__

无法配置

__实现逻辑︰__

基于错误代码 / 元数据服务的响应标头中传递

__电路中断︰__

根据[断路器](http://msdn.microsoft.com/library/dn589784.aspx)

## 可扩展性

可以提供自定义重试策略实现的公共接口

```csharp
public interface IRetryPolicy
{
    /// <summary>
    /// Generates a new retry policy for the current request attempt.
    /// </summary>
    IRetryPolicy CreateInstance();

    /// <summary>
    /// Determines whether the operation should be retried and the interval until the next retry.
    /// </summary>
    /// <param name="currentRetryCount">An integer specifying the number of retries for the given operation. A value of zero signifies this is the first error encountered.</param>
    /// <param name="statusCode">An integer containing the status code for the last operation.</param>
    /// <param name="retryInterval">A <see cref="TimeSpan"/> indicating the interval to wait until the next retry.</param>
    /// <returns><c>true</c> if the operation should be retried; otherwise, <c>false</c>.</returns>
    bool ShouldRetry(int currentRetryCount, int statusCode, out TimeSpan retryInterval);
}
```

## 遥测

使用 EventSource 的 ETW 事件登录重试次数。 以下是应为每次重试尝试记录的字段

参数            | 说明
-------------------- | -------------------------------------------------------
了申请 Id | ""
policyType | ""RetryExponential
操作 | "获取︰ https://retry-guidance-tests.servicebus.windows.net/TestQueue/?api-version=2014-05"
operationStartTime | "2014/5/9 日下午 10:00:13"
operationEndTime | "2014/5/9 日下午 10:00:14"
迭代 | ""0
iterationSleep | ""00:00:00.1000000
lastExceptionType | ""Microsoft.ServiceBus.Messaging.MessagingCommunicationException
exceptionMessage | "无法解析此远程名称: '重试的指南-tests.servicebus.windows.net。TrackingId:6a26f99c-dc6d-422e-8565-f89fdd0d4fe3，TimeStamp:9/5/2014年下午 10:00:13"

测试
