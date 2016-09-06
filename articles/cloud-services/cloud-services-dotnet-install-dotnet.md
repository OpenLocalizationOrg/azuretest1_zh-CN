---
ms.openlocfilehash: 8786acc2fdfd36d294b65ed65274cceff026b593
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在云服务角色上安装.NET"
   description="本文介绍如何手动安装.NET 框架，在云服务 Web 和辅助角色"
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/03/2015"
   ms.author="saurabh"/>

# 在云服务角色上安装.NET 

本文介绍如何在云服务 Web 和辅助角色上安装.NET 框架。 可以使用下列步骤在 Azure 来宾操作系统家族 4 上安装.NET framework 4.5.2 或.NET 4.6。 来宾操作系统发行版的最新信息请参阅[Azure 来宾操作系统释放和 SDK 兼容性矩阵](cloud-services-guestos-update-matrix.md)。

作为您的云项目和作为该角色的启动任务的一部分启动安装程序的一部分的.NET 安装程序包包括涉及安装.NET web 和辅助角色的过程。  

## 向项目中添加.NET 安装程序
1. 下载的 web 安装程序要安装.NET 框架
    - [.NET 4.5.2 web 安装程序](http://go.microsoft.com/fwlink/p/?LinkId=397703)
    - [.NET 4.6 web 安装程序](http://go.microsoft.com/fwlink/?LinkId=528259)
2. 为 Web 角色
  1. 在**解决方案资源管理器**中下云服务项目中的**角色**中右键单击您的角色并选择**添加 > 新建文件夹**。 创建一个名为*bin*的文件夹
  2. **Bin**文件夹中用鼠标右键单击并选择**添加 > 现有项**。 选择.NET 安装程序并将其添加到 bin 文件夹。
3. 工作角色
  1. 右键单击您的角色并选择**添加 > 现有项**。 选择.NET 安装程序并将其添加到该角色。 

这样的角色内容文件夹会自动添加到云服务包和部署到虚拟机上的一个固定位置，添加的文件。 在云服务中的所有 web 和辅助角色都重复此过程，以便所有角色都已安装程序的一个副本。

![角色的安装程序文件的文件的内容][1]

## 定义您的角色的启动任务
启动任务允许您在角色启动之前执行的操作。 作为启动任务的一部分安装.NET Framework 将确保任何应用程序代码运行之前，安装了框架。 了解更多有关启动任务，请参见︰ [Azure 中运行启动任务](https://msdn.microsoft.com/library/azure/hh180155.aspx)。 

1. 所有角色的**WebRole**或**WorkerRole**节点下的*ServiceDefinition.csdef*文件中添加以下︰
    
    ```xml
     <LocalResources>
        <LocalStorage name="InstallLogs" sizeInMB="5" cleanOnRoleRecycle="false" />
     </LocalResources>
     <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
            <Variable name="PathToInstallLogs">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='InstallLogs']/@path" />
            </Variable>
            </Environment>
        </Task>
     </Startup>
    ```

    上述的配置将使用管理员权限运行控制台命令*install.cmd* ，以便它可以安装.NET 框架。 配置还会创建 LocalStorage 与*InstallLogs*存储通过安装脚本创建的任何日志信息的名称。 有关详细信息，请参阅︰[使用本地存储区来存储文件在启动时](https://msdn.microsoft.com/library/azure/hh974419.aspx) 

2. 创建**install.cmd**文件并将其添加到所有角色的角色上单击鼠标右键并选择**添加 > 现有项...**。 因此，.NET 安装程序文件，以及 install.cmd 文件，现在应具有的所有角色。
    
    ![角色使用的所有文件的内容][2]

    > [AZURE.NOTE] 使用记事本等纯文本编辑器创建此文件。 如果使用 Visual Studio 创建一个文本文件，然后将其重命名为 '.cmd 该文件可能仍然包含 utf-8 字节顺序标记并运行该脚本的第一行将导致错误。 如果要使用 Visual Studio 创建文件保留添加到该文件的第一行 REM （批注），以便在运行时它将被忽略。 

3. 将以下脚本添加到**install.cmd**文件中︰

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    set netfx="NDP452"
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP46" goto NDP46
        set netfxinstallfile="NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    :NDP46
    set netfxinstallfile="NDP46-KB3045560-Web.exe"
    set netfxregkey="0x60051"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set startuptasklog=%PathToInstallLogs%startuptasklog-%timestamp%.txt
    set netfxinstallerlog=%PathToInstallLogs%NetFXInstallerLog-%timestamp%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release | Find %netfxregkey%
    if %ERRORLEVEL%== 0 goto end
    
    REM ***** Installing .NET *****
    echo Installing .NET. Logfile: %netfxinstallerlog% >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% >> %startuptasklog% 2>>&1
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    ```
    > [AZURE.IMPORTANT] 更新以匹配您想要安装的框架版本的脚本*netfx*变量的值。 若要安装.NET 4.5.2 *netfx*变量应设置为*"NDP452"* ，安装.NET 4.6 *netfx*变量应设置为*"NDP46"*
        
    安装脚本会检查是否指定的.NET framework 版本已安装在计算机上通过查询注册表。 如果未安装.NET 版本则是.Net Web 安装程序已启动。 为帮助解决任何问题脚本将到名为*startuptasklog-(currentdatetime).txt* *InstallLogs*本地存储区中存储的文件记录所有的活动。
 
      

## 配置诊断程序来传输启动任务日志到 blob 存储 （可选）
您也可以配置 Azure 诊断转移任何启动脚本或 blob 存储.NET 安装程序生成的日志文件。 使用这种方法可以通过只需从 blob 存储下载日志文件，而不必到远程桌面角色来查看日志。

配置诊断程序打开*diagnostics.wadcfgx*和添加的**目录**节点下的下列︰ 

```xml 
<DataSources>
    <DirectoryConfiguration containerName="netfx-install">
    <LocalResource name="InstallLogs" relativePath="."/>
    </DirectoryConfiguration>
</DataSources>
```

这将配置 azure 诊断转移到诊断存储帐户*netfx 安装*blob 容器中的*InstallLogs*资源中的所有文件。

## 部署服务 
部署您的服务时启动任务将运行，如果未安装，请安装.NET 框架。 而框架正在安装并可能甚至如果框架安装需要重新启动您的角色都处于繁忙状态。 


## 其他资源

- [安装.NET Framework][]
- [如何︰ 确定是否已安装的.NET Framework 版本][]
- [.NET Framework 安装的疑难解答][]

[如何︰ 确定是否已安装的.NET Framework 版本]: https://msdn.microsoft.com/library/hh925568.aspx
[安装.NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[.NET Framework 安装的疑难解答]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 

测试
