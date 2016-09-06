---
ms.openlocfilehash: da409ff4af25733b55ff325707c6d522e03cdbec
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在 Azure 服务结构中的有状态服务安全复制流量"
   description="启用复制后，有状态服务将其状态从主要副本复制到次要副本，此类通信需要受到保护，防止窃听和篡改。"
   services="service-fabric"
   documentationCenter=".net"
   authors="leikong"
   manager="vipulm"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/13/2015"
   ms.author="leikong"/>

# 确保复制通信的安全

当启用复制时，有状态服务整个副本中复制状态。 此页面是如何对此类通信量配置保护。

有两种类型的支持的安全设置︰

- X509︰ 使用 X509 证书，以确保通信通道的安全。 希望用户对 ACL 证书私钥允许参与者/服务主机进程能够使用证书凭据。
- 窗口︰ 使用 windows 安全 （需要 Active Directory） 确保通信通道的安全。

下面一节演示如何配置上述两种类型的设置。
配置"CredentialType"可以确定正在使用哪种类型。

## CredentialType = X509

### 配置名称

|名称|备注|
|----|-------|
|StoreLocation|存储位置中查找证书︰ CurrentUser 或 LocalMachine|
|StoreName|要查找证书的存储区名称|
|FindType|标识在 FindValue 参数中提供的值的类型︰ FindBySubjectName 或 FindByThumbPrint|
|FindValue|用于查找证书搜索目标|
|AllowedCommonNames|复制器所使用证书公用名称/dns 名称的逗号分隔列表。|
|IssuerThumbprints|颁发者的证书指纹的逗号分隔列表。 指定时，如果它由列表中的条目之一颁发的验证传入的证书。 始终执行链验证。|
|RemoteCertThumbprints|一个逗号分隔的列表复制器所使用的证书指纹。|
|保护级别|指示如何保护数据︰ 符号或 EncryptAndSign 或无|

## CredentialType = Windows

### 配置名称

|名称|备注|
|----|-------|
|ServicePrincipalName|为此服务注册服务主体名称。 可以为空如果作为计算机帐户运行的服务/演员主机进程 (例如︰ 网络服务本地系统等。)|
|WindowsIdentities|以逗号分隔的所有使用者服务/主机进程的 windows 标识的列表。
|保护级别|指示如何保护数据︰ 符号或 EncryptAndSign 或无|

## 示例

### 示例 1: CredentialType = X509

```xml
<Section Name="SecurityConfig">
  <Parameter Name="CredentialType" Value="X509" />
  <Parameter Name="FindType" Value="FindByThumbprint" />
  <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
  <Parameter Name="StoreLocation" Value="LocalMachine" />
  <Parameter Name="StoreName" Value="My" />
  <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
  <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
</Section>
```

### 示例 2: CredentialType = Windows
所有服务/演员主机进程作为网络服务或本地系统都运行时，此代码段显示示例。 ServicePrincipalName 是空的。

```xml
<Section Name="SecurityConfig">
  <Parameter Name="CredentialType" Value="Windows" />
  <Parameter Name="ServicePrincipalName" Value="" />
  <!--This machine group contains all machines in a cluster-->
  <Parameter Name="WindowsIdentities" Value="redmond\ClusterMachineGroup" />
  <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
</Section>
```

### 示例 3: CredentialType = Windows
所有服务/演员主机进程运行作为一个组管理与有效 ServicePrincipalName serice 帐户时，此代码段显示示例。

```xml
<Section Name="SecurityConfig">
  <Parameter Name="CredentialType" Value="Windows" />
  <Parameter Name="ServicePrincipalName" Value="servicefabric/cluster.microsoft.com" />
  <--All actor/service host processes run as redmond\GroupManagedServiceAccount-->
  <Parameter Name="WindowsIdentities" Value="redmond\GroupManagedServiceAccount" />
  <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
</Section>
```
 