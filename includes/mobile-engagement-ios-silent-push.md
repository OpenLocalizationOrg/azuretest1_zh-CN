---
ms.openlocfilehash: 260735a66d6fce1a7f76e706e6ebd5b1212dfeae
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
> [AZURE.IMPORTANT] 要从移动服务中接收推送通知，您需要启用`Silent Remote Notifications`应用程序中。 您需要将远程通知值添加到 Info.plist 文件中的 UIBackgroundModes 数组。

1. 打开`info.plist`项目中的文件
2. 右键单击该列表顶部的项目 (`Information Property List`)，并添加新行

    ![][1]

3. 在新的一行中输入 `Required background modes`

    ![][2]

4. 单击左箭头，可展开的行
5. 将下面的值添加到项 0 `App downloads content in response to push notifications`

    ![][3]

一旦进行了更改，则 info.plist XML 应包含下面的键和值︰

    <key>UIBackgroundModes</key>
        <array>
            <string>remote-notification</string>
        </array>
    ...

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png
[2]: ./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png
[3]: ./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png
