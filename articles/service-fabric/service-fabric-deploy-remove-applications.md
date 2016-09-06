---
ms.openlocfilehash: 0b37083fd9b5a8c17e71bc278a6c5b267ca93487
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="服务结构的应用程序部署"
   description="如何部署和服务结构中删除应用程序"
   services="service-fabric"
   documentationCenter=".net"
   authors="alexwun"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/03/2015"
   ms.author="alexwun"/>

# 将应用程序部署

一次[已打包应用程序类型]的[10]，它已准备部署到群集服务结构。 部署包括以下三个步骤︰

1. 上载应用程序包
2. 注册应用程序类型
3. 创建应用程序实例

>[AZURE.NOTE] 如果您使用 Visual Studio 进行部署和调试本地开发群集上的应用程序，如下所述的步骤的所有自动处理通过调用应用程序项目的脚本文件夹中找到的 PowerShell 脚本。 本文提供了有关这些脚本怎么做的以便可以执行相同的操作，在 Visual Studio 之外的背景。

## 将上载应用程序包

上载应用程序包将其放入一个位置访问内部服务结构组件，并可以通过 PowerShell。 在运行之前任何 PowerShell 命令在这篇文章，总是入手，首次连接到服务结构群集使用**连接 ServiceFabricCluster**。

假设您有一个名为*MyApplicationType*的文件夹包含必需的应用程序清单、 服务清单和代码配置/数据的包，然后**复制 ServiceFabricApplicationPackage**命令将上载程序包。 例如︰

~~~
PS D:\temp> dir

    Directory: D:\temp

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----         3/19/2015   8:11 PM            MyApplicationType

PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Copy-ServiceFabricApplicationPackage MyApplicationType
Copy application package succeeded

PS D:\temp>
~~~

## 注册应用程序包

注册应用程序软件包，使得应用程序类型和用于声明中可用的应用程序清单的版本。 系统将读取包在上一步中上载、 验证包 （相当于本地运行**测试 ServiceFabricApplicationPackage** ），处理程序包的内容，并将处理的软件包复制到内部系统的位置。

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

**注册 ServiceFabricApplicationType**命令返回仅后由系统成功复制了应用程序包。 这花费的时间取决于应用程序软件包的内容。 可以用**-TimeoutSec**参数如果需要提供更长的超时 （默认超时值为 60 秒）。

**获得 ServiceFabricApplicationType**命令将列出已成功注册的应用程序类型的所有版本。

## 创建应用程序

应用程序可以使用任何已登记使用**新的 ServiceFabricApplication**命令成功的应用程序类型版本进行实例化。 每个应用程序的名称必须与启动*结构︰*方案，它是唯一每个应用程序实例。 如果目标应用程序类型的应用程序清单中定义任何默认服务，然后那些将还创建这一次。

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

**Get ServiceFabricApplication**命令会列出所有已成功创建及其总体状态的应用程序实例。

**获得 ServiceFabricService**命令可以列出所有服务实例已成功创建一个给定的应用程序实例中。 默认服务 （如果有的话） 将在此处列出。

对于任何给定版本的已注册应用程序类型，可以创建多个应用程序实例。 每个应用程序实例将以隔离方式，与自己的工作目录和进程运行。

## 删除应用程序

当不再需要应用程序实例时，它可以被永久删除使用**删除 ServiceFabricApplication**命令。 这将自动删除属于也，永久删除所有服务状态的应用程序的所有服务。 无法撤消此操作和应用程序状态不能恢复。

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

当不再需要某一特定版本的应用程序类型时，它应该是未注册使用**注销 ServiceFabricApplicationType**命令。 正在撤消注册未使用的类型将发行映像存储在该类型的应用程序包内容所使用的存储空间。 只要没有实例化对其或待定引用它的应用程序升级的应用程序，可以是未注册的应用程序类型。

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

<!--
## Next steps

TODO [Upgrade applications][11]
-->

## 疑难解答

### 复制 ServiceFabricApplicationPackage 索要 ImageStoreConnectionString

服务结构 SDK 环境应该已经设置好正确的默认设置。 但是，如果需要所有命令 ImageStoreConnectionString 应与正在使用服务结构群集中，可以使用**Get ServiceFabricClusterManifest**命令检索群集清单中找到的值相匹配︰

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## 下一步行动

[我们的服务结构应用程序升级](service-fabric-application-upgrade.md)

[服务结构状况简介](service-fabric-health-introduction.md)

[诊断并解决服务结构服务](service-fabric-diagnose-monitor-your-service-index.md)

[模型服务结构中的应用](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
 