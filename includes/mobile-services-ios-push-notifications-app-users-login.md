---
ms.openlocfilehash: 78b2753f623d0c87c916e310d2dc14a8e9036a54
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

接下来，您需要更改推式通知已注册的方式，以便用户进行身份验证之后尝试注册。

1. 在**QSAppDelegate.m**中，完全删除**didFinishLaunchingWithOptions**的实现。

2. 打开**QSTodoListViewController.m**和**viewDidLoad**方法的末尾添加以下代码︰

```
// Register for remote notifications
[[UIApplication sharedApplication] registerForRemoteNotificationTypes:
UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
```
