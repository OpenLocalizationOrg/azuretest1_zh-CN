---
ms.openlocfilehash: 67ac97abaa33fea39fc42dea19da39a2cd800e4f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 的云服务的业务定义和服务配置的 XML 证书" 
    description="了解如何配置证书服务定义和配置文件中。" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/24/2015"
    ms.author="adegeo"/>



# 证书中配置的服务定义和配置

运行 web 或辅助角色的虚拟机可以有与它们关联的证书。 一旦您已上载到门户的您的证书，您需要配置的服务定义 (.csdef) 和服务 (.cscfg) 配置文件的证书。 

安装完之后，虚拟机可以访问该证书的私钥。 因此，您可以限制对流程以提升的权限的访问。 

## 服务定义示例

这里是证书的一种在服务定义中定义。

```xml
<ServiceDefinition name="WindowsAzureProject4" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="MyWokerRole"> <!-- or <WebRole name="MyWebRole" vmsize="Small"> -->
    <ConfigurationSettings>
      ...
    </ConfigurationSettings>
    <Certificates>
      <Certificate name="MySSLCert" storeLocation="LocalMachine" storeName="My" permissionLevel="elevated" />
    </Certificates>
  </WorkerRole>
</ServiceDefinition>
```

### 权限
权限 (`permisionLevel`属性) 可以设置为以下之一︰

| 权限值  | 说明 |
| ----------------  | ----------- |
| limitedOrElevated | **（默认值）**所有角色进程可以都访问的专用密钥。 |
| 提升          | 已提升的进程可以访问的专用密钥。|

## 服务配置示例

这里是证书的一种在服务配置文件中定义。

```xml
<Role name="MyWokerRole">
...
    <Certificates>
        <Certificate name="MySSLCert" 
            thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
            thumbprintAlgorithm="sha1" />
    </Certificates>
...
</Role>
```

**请注意**，匹配`name`属性。

## 下一步行动
查看[服务定义 XML](https://msdn.microsoft.com/library/azure/ee758711.aspx)架构和[服务配置 XML](https://msdn.microsoft.com/library/azure/ee758710.aspx)架构。
测试
