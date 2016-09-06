---
ms.openlocfilehash: 371adc2a38bf797146abb6745b7fc87f226f3f76
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 对于 Java-手动下载的 azure 的客户端库

在[Apache 许可证、 2.0 版]的[许可证]下分布 Azure 的 Java 库。 单击[此处][zip 下载]的 ZIP 文件库及其所有依赖项。  这可通过 Microsoft 开放技术，inc.保留所有权利。 请参阅 license.txt 和 ThirdPartyNotices.txt 文件内 ZIP 许可以及其他信息。

## 对于 Java 的 Maven azure 库

如果已将项目设置为生成使用 Maven，pom.xml 文件中添加以下依存关系。 注意︰ 在 Eclipse 的 Azure 库用于 Java 创建 Maven 项目有关的信息，请参阅[http://go.microsoft.com/fwlink/?LinkId=622998]()。

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-compute</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-network</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-sql</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-storage</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-websites</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-media</artifactId>
        <version>0.6.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>0.6.0</version>
    </dependency>


在`<version>`元素，在此示例中的版本编号替换为有效的版本号，可以从[Azure 上 Maven 存储库库中](http://go.microsoft.com/fwlink/?LinkID=286274)获得。

[许可证]: http://www.apache.org/licenses/LICENSE-2.0.html
[zip 下载]:  http://go.microsoft.com/fwlink/?LinkId=253887
