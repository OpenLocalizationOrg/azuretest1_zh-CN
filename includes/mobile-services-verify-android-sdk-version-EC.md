---
ms.openlocfilehash: ea014cb8a93815bd79949a2a4f12c6588ee69e7d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
由于持续的发展，在 Eclipse 安装 Android SDK 版本可能不匹配的版本在代码中。 在本教程中引用 Android SDK 是版本 21 日最新的在写作的时候。 新的 SDK 版本中出现，而我们建议使用最新版本可能会增加版本号。

两个版本不匹配的症状有︰

1. 看起来在 Eclipse 控制台的底部窗格中。 您可能会看到错误消息"**无法解决 android n 目标**"窗体。

2. 标准的 Android 对象中应解决的代码根据`import`语句可能会生成错误消息。

如果其中一种出现，Android SDK 安装在 Eclipse 中的版本可能不匹配 SDK 下载的项目的目标。  若要验证的版本，请进行以下更改︰


1. 在 Eclipse 中，单击**窗口**，然后单击**Android SDK 管理器**。 如果您没有安装最新版本的 SDK 平台，然后单击安装。 记下版本号。

2. 打开项目文件**AndroidManifest.xml**。 确保在**使用 sdk**元素中， **targetSdkVersion**设置为安装最新版本。 **使用 sdk**标签可能如下所示︰
 
            <uses-sdk
                android:minSdkVersion="8"
                android:targetSdkVersion="21" />
    
3. 日蚀式包资源管理器中右键单击项目节点，选择**属性**，在左边列中选择**Android**。 确保**项目生成目标**设置为同一 SDK 版本为**targetSdkVersion**。