---
ms.openlocfilehash: 563edd533a104efbeb8fbc7da32df24679bfadbe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="可靠的演员计时器和提醒"
   description="计时器和简介提醒的服务结构可靠参与者。"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2015"
   ms.author="amanbha"/>


# 主角的计时器
主角计时器提供.NET 计时器的简单包装，以便回调方法尊重演员运行时提供的基于打开的并发性保证。

参与者可以使用`RegisterTimer`和`UnregisterTimer`上注册和注销其计时器其基类的方法。 下面的示例演示如何使用计时器 Api。 Api 是非常类似于.NET 计时器。 在计时器到期时下面的示例`MoveObject`将由演员运行时调用方法，并保证尊重基于打开的并发性，这意味着没有其他演员方法或计时器/提醒回调将正在此回调之前完成执行。

```csharp
class VisualObjectActor : Actor<VisualObject>, IVisualObject
{
    private IActorTimer _updateTimer;

    public override Task OnActivateAsync()
    {
        ...

        _updateTimer = RegisterTimer(
            MoveObject,                     // callback method
            state,                          // state to pass to callback method
            TimeSpan.FromMilliseconds(15),  // amount of time to delay before callback is invoked
            TimeSpan.FromMilliseconds(15)); // time interval between invocation of the callback method

        return base.OnActivateAsync();
    }

    public override Task OnDeactivateAsync()
    {
        if (_updateTimer != null)
        {
            UnregisterTimer(_updateTimer);
        }

        return base.OnDeactivateAsync();
    }

    private Task MoveObject(object state)
    {
        ...
        return TaskDone.Done;
    }
}
```

下一个期间的计时器启动后回调完成执行。 这意味着回调执行和完成回调后启动时，停止计时器。

当回调完成主角是否像在上面的示例状态演员，演员运行时保存主角状态。 如果在保存状态发生错误，将停用该参与者对象，新实例将被激活。 可以将不会修改参与者状态的回调方法注册为只读的计时器回调通过计时器回调，在指定的只读属性[只读方法](service-fabric-reliable-actors-introduction.md#readonly-methods)上节中所述。

当参与者被作为垃圾回收的一部分停用并没有计时器回调被调用后，停止了所有计时器。 另外，演员运行时不会保留任何信息之前停用正在运行的计时器。 由使用者注册时它就会在将来重新激活所需的任何计时器。 有关详细信息，请参阅部分[演员垃圾回收](service-fabric-reliable-actors-lifecycle.md)。

## 主角的提醒
提醒是一种机制来触发持续回调上演员在指定时间。 它们的功能是类似于计时器，而与不同的计时器提醒会触发在所有情况下直到提醒显式注销的主角。 具体来说，提醒触发整个演员 deactivations 和故障切换因为演员运行时仍然存在信息使用者的提醒。

仅有状态的参与者支持提醒。 无状态的参与者不能使用提醒。 参与者的状态提供程序负责存储有关已登记参与者的提醒信息。  

注册提醒使用者调用`RegisterReminder`方法提供基类，如下面的示例中所示。

```csharp
string task = "Pay cell phone bill";
int amountInDollars = 100;
Task<IActorReminder> reminderRegistration = RegisterReminder(
                                                task,
                                                BitConverter.GetBytes(amountInDollars),
                                                TimeSpan.FromDays(3),
                                                TimeSpan.FromDays(1),
                                                ActorReminderAttributes.None);
```

在上例中，`"Pay cell phone bill"`是提醒的名称，它是一个字符串，使用者用来唯一标识提醒。 `BitConverter.GetBytes(amountInDollars)` 是提醒与相关联的上下文。 它将被传递回主角作为参数给提醒回调，即`IRemindable.ReceiveReminderAsync`。

使用提醒的参与者必须实现`IRemindable`接口，如下面的示例中所示。

```csharp
public class ToDoListActor : Actor<ToDoList>, IToDoListActor, IRemindable
{
    public Task ReceiveReminderAsync(string reminderName, byte[] context, TimeSpan dueTime, TimeSpan period)
    {
        if (reminderName.Equals("Pay cell phone bill"))
        {
            int amountToPay = BitConverter.ToInt32(context, 0);
            System.Console.WriteLine("Please pay your cell phone bill of ${0}!", amountToPay);
        }
        return Task.FromResult(true);
    }
}
```

触发提醒时，将调用运行时结构者`ReceiveReminderAsync`在主角的方法。 操作者可以注册多个提醒和`ReceiveReminderAsync`的那些提醒任何触发任何时候调用方法。 参与者可以使用传递到中提醒名称`ReceiveReminderAsync`方法来算出触发的提醒。

运行时保存主角演员状态下`ReceiveReminderAsync`调用完成。 如果在保存状态发生错误，将停用该参与者对象，新实例将被激活。 若要指定状态不需要提醒回调，在完成时保存`ActorReminderAttributes.ReadOnly`可以在中设置标志`attributes`参数时`RegisterReminder`调用方法来创建提醒。

要注销提醒， `UnregisterReminder` ，应调用方法，如下面的示例中所示。

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminder(reminder);
```

如上所示，`UnregisterReminder`方法接受`IActorReminder`接口。 基类支持的演员`GetReminder`方法可用于检索`IActorReminder`通过传入提醒名称的接口。 这很方便，因为不需要保留主角`IActorReminder`从返回的接口`RegisterReminder`方法调用。
