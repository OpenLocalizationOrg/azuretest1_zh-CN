---
ms.openlocfilehash: 2a668e119c828f4ad45bb56b545ccc9368c686ae
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="服务结构服务清单资源"
   description="如何描述服务清单中的资源"
   services="service-fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="anuragg"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/26/2015"
   ms.author="sumukhs"/>

# 服务清单资源

## 概述

服务清单允许服务使用而不会更改编译后的代码是声明更改的资源。 服务结构支持终结点资源的配置的服务。 可以通过为应用程序清单 SecurityGroup 控制对服务清单中指定的资源的访问权限。 资源的声明使这些资源以在部署时更改时间而不需要的服务，以引入新的配置机制。

## 终结点

当服务清单中定义的终结点资源时，服务结构将从保留应用程序端口范围分配端口。 此外，服务还可以请求为某个资源在某个特定的端口。 服务在不同的群集节点上运行的复制副本可以不同的端口号分配时所在的节点上运行相同的服务的副本共享同一个端口。 这些端口可以供各种用途，如复制、 侦听客户端请求等服务副本。

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint" Protocol="http"/>
    <Endpoint Name="ServiceInputEndpoint" Protocol="http" Port="80"/>
    <Endpoint Name="ReplicatorEndpoint" Protocol="tcp"/>
  </Endpoints>
</Resources>
```

请参阅[配置有状态的可靠服务](../Service-Fabric/service-fabric-reliable-services-configuration.md)读取有关引用的配置包设置终结点的详细信息文件 (settings.xml)。

## 示例
下面的服务清单定义 1 个 TCP 终结点资源和 2 中的 HTTP 端点资源&lt;资源&gt;元素。

HTTP 端点自动是 ACL 的服务结构。

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="SP1" Version="V1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Description>Test Service</Description>
  <ServiceTypes>
    <StatefulServiceType ServiceTypeName="PersistType" HasPersistedState="true" />
  </ServiceTypes>
  <CodePackage Name="CP1" Version="V1">
    <EntryPoint>
      <ExeHost>
        <Program>CB\Code.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="CP1.Config0" Version="V1" />
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint" Protocol="http"/>
      <Endpoint Name="ServiceInputEndpoint" Protocol="http" Port="80"/>
      <Endpoint Name="ReplicatorEndpoint" Protocol="tcp"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
 
