---
ms.openlocfilehash: 66553907519f0d3fd5885cb8241aee3a607ed944
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 在您的 Mac，打开 Xcode **QSTodoListViewController.m**并添加下面的方法。 如果您没有为您标识提供程序使用 Facebook， _facebook_更改为_microsoftaccount_，_使用 twitter_， _google_或_windowsazureactivedirectory_ 。

        - (void) loginAndGetData
        {
            MSClient *client = self.todoService.client;
            if (client.currentUser != nil) {
                return;
            }

            [client loginWithProvider:@"facebook" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                [self refresh];
            }];
        }

2. 更换`[self refresh]`在`viewDidLoad`如下︰

        [self loginAndGetData];

3. 请按**运行**以启动该应用程序，然后再登录。 当您登录时，您应该能够查看 Todo 列表并进行更新。
