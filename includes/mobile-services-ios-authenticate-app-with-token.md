---
ms.openlocfilehash: 883f22dafbe3812e5ae74d21a4615846944441e7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

前面的示例联系人标识提供程序和移动服务每次启动应用程序时。 相反，您可以缓存授权令牌并尝试首先使用它。

* 加密并存储在 iOS 客户端的身份验证令牌的推荐的方式是使用 iOS 钥匙链。 我们将使用[SSKeychain](https://github.com/soffes/sskeychain) -iOS 钥匙链的简单包装。 按照 SSKeychain 页面上的说明并将其添加到您的项目。 验证启用了**启用模块**设置中的项目**生成设置**（部分**苹果 LLVM 的语言-模块**）。

* 打开**QSTodoListViewController.m** ，然后添加以下代码︰

```
        - (void) saveAuthInfo {
                [SSKeychain setPassword:self.todoService.client.currentUser.mobileServiceAuthenticationToken forService:@"AzureMobileServiceTutorial" account:self.todoService.client.currentUser.userId]
        }


        - (void)loadAuthInfo {
                NSString *userid = [[SSKeychain accountsForService:@"AzureMobileServiceTutorial"][0] valueForKey:@"acct"];
            if (userid) {
                NSLog(@"userid: %@", userid);
                self.todoService.client.currentUser = [[MSUser alloc] initWithUserId:userid];
                 self.todoService.client.currentUser.mobileServiceAuthenticationToken = [SSKeychain passwordForService:@"AzureMobileServiceTutorial" account:userid];

            }
        }
```

* 在`loginAndGetData`，修改`loginWithProvider:controller:animated:completion:`的完成块。 添加以下行右前的`[self refresh]`来存储用户 ID 和令牌的属性︰

```
                [self saveAuthInfo];
```

* 让应用程序启动时加载的用户 ID 和标记。 在`viewDidLoad` **QSTodoListViewController.m**，在添加后的该立即`self.todoService`被初始化。

```
                [self loadAuthInfo];
```
