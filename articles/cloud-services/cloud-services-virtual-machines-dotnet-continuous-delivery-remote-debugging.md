---
ms.openlocfilehash: 2d3a1ee0acb7d197f3a50d79953d24813951f147
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="启用远程调试，并不断提供"
    description="了解如何启用远程调试使用不断提供可用于部署到 Azure 时"
    services="cloud-services"
    documentationCenter=".net"
    authors="kempb"
    manager="douge"
    editor="tglee"/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/09/2015"
    ms.author="kempb"/>
# 启用远程调试使用不断提供发布到 Azure 时

您可以启用远程调试在 Azure 时[不断提供可](cloud-services-dotnet-continuous-delivery.md)用于发布到 Azure 按照下面的步骤。

本主题︰

[启用远程调试的云服务](#cloudservice)

[启用虚拟机的远程调试](#virtualmachine)

## <a name="cloudservice"></a>启用远程调试的云服务

1. 在生成代理上设置初始环境 Azure [Azure 的生成的命令行](http://msdn.microsoft.com/library/hh535755.aspx)中所述。
2. 因为远程调试运行 (msvsmon.exe) 是必需的软件包，安装[的 Visual Studio 2015 RC 远程工具](http://www.microsoft.com/download/details.aspx?id=46874)（或[远程工具 Visual Studio 2013年更新 5 RC](https://www.microsoft.com/en-us/download/details.aspx?id=46870)如果您使用的 Visual Studio 2013年）。 作为一种替代方法，您可以从已安装的 Visual Studio 的系统复制的远程调试二进制文件。
3. [创建的 Azure 服务证书](http://msdn.microsoft.com/library/azure/gg432987.aspx)中所述创建一个证书。 保留的.pfx 和 RDP 证书指纹并将证书传到目标云服务。
4. 在 MSBuild 命令行中使用下列选项来构建和打包与启用远程调试。 （替代实际角度带括号的项目系统和项目文件的路径）。

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.6" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath` Visual Studio 的文件夹路径包含 msvsmon.exe 在远程工具。

5. 使用上一步中生成的包和.cscfg 文件发布到目标云服务。
6. 证书 （.pfx 文件） 导入到已使用 Azure SDK 的 Visual Studio.net 安装的计算机中。

## <a name="virtualmachine"></a>启用虚拟机的远程调试

1. 创建 Azure 的虚拟机。 请参阅[创建运行 Windows 服务器的虚拟机](../virtual-machines-windows-tutorial.md)或[创建 Visual Studio 中的 Azure 虚拟机](http://msdn.microsoft.com/library/azure/dn569263.aspx)。
2. 在[Azure 的门户页面](http://go.microsoft.com/fwlink/p/?LinkID=269851)上，查看请参阅"RDP 证书指纹"虚拟机虚拟机操控板。 这用于`ServerThumbprint`扩展配置中的值。
3. [创建的 Azure 服务证书](http://msdn.microsoft.com/library/azure/gg432987.aspx)中所述创建一个客户端证书 （保留.pfx 和 RDP 证书指纹）。
4. 安装[Azure Powershell](http://go.microsoft.com/?linkid=9811175&clcid=0x409) (版本 0.7.4 或更高版本) 从 Microsoft 下载中心获取。
5. 运行下面的脚本来启用 RemoteDebug 扩展。 替换路径和个人数据与您自己的比如预订名称、 服务名称和指纹。 (注︰ 此脚本被配置为 Visual Studio 2015 RC。 如果您使用的 Visual Studio 2013年，使用"RemoteDebugVS2013"引用名称和 ExtensionName。

    <pre>
    添加 AzureAccount

    选择 AzureSubscription"Microsoft 订阅"

    $vm = 获取 AzureVM 服务名"mytestvm1"的名称"mytestvm1"

    $endpoints = @(，@{名称 ="RDConnVS2013";PublicPort = 30400;PrivatePort = 30398}，@{名称 ="RDFwdrVS2013";PublicPort = 31400;PrivatePort = 31398})

    foreach (在 $endpoints $endpoint) {虚拟机添加 AzureEndpoint-$vm-命名为 $endpoint。Name-tcp 协议-PublicPort $endpoint。PublicPort-本地 $endpoint。} PrivatePort

    $referenceName ="Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015"$publisher ="Microsoft.VisualStudio.WindowsAzure.RemoteDebug"$extensionName ="RemoteDebugVS2015"$version ="1.*"$publicConfiguration ="<PublicConfig>< Connector.Enabled > 真 < /Connector.Enabled ><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm |一组 AzureVMExtension `
    -ReferenceName $referenceName `
    -出版商 $publisher `
    -ExtensionName $extensionName `
    -$version 版本-PublicConfiguration $publicConfiguration

    foreach ($extension 在 $vm 中。虚拟机。ResourceExtensionReferences) {如果 (($extension.引用名称-eq $referenceName) `
    -and ($extension.Publisher -eq $publisher) `
    -和 ($extension。名称-eq $extensionName）-和 ($extension。版本的 eq $version)) {$extension。ResourceExtensionParameterValues [0]。键 = 'config.txt' 中断}}

    $vm |更新 AzureVM
    </pre>

6. (.Pfx) 将证书导入到已使用 Azure SDK 的 Visual Studio.net 安装的计算机中。
 
测试
