---
ms.openlocfilehash: 792dfadf3ac36536724232c63b52fd47f893d4c7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
 pageTitle="使用 JDBC 查询在 Azure HDInsight 配置单元"
 description="了解如何使用 JDBC 连接到 Azure HDInsight 配置单元和远程存储在云中的数据上运行查询。"
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="paulettm"
 editor="cgronlun"
    tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="java"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="07/17/2015"
 ms.author="larryfr"/>

#连接到在 Azure HDInsight 使用的配置单元的 JDBC 驱动程序的配置单元

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

在本文中，您将学习如何从 Java 应用程序使用 JDBC 远程提交到 HDInsight 群集配置单元查询。 配置单元 JDBC 接口的详细信息，请参阅[HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface)。

##先决条件

若要完成本文中的步骤操作，您需要︰

* Hadoop HDInsight 群集上。 基于 linux * 的或基于 Windows 群集将起作用。

* [Java 开发人员工具箱 (JDK) 版本 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)或更高版本。

* [Apache Maven](https://maven.apache.org)。 Maven 是项目生成的 Java 项目系统，由与此项目关联的项目。

##连接字符串

JDBC 连接到 HDInsight 群集在 Azure 上进行超过 443，和使用 SSL 来保护通信。 坐在后面的群集公用网关将通信重定向到 HiveServer2 实际上正在侦听的端口。 因此，典型的连接字符串将类似以下内容︰

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=/hive2

##身份验证

建立连接时，您必须指定 HDInsight 群集管理员名称和密码。 这些身份验证请求到网关。 例如，下面的 Java 代码将打开一个新的连接使用的连接字符串、 管理员名和密码︰

    DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);

##查询

建立连接后，您可以对配置单元运行查询。 例如，下面的 Java 代码执行表，将结果限制为仅三行，从__选择__一个，然后显示结果︰

    sql = "SELECT querytime, market, deviceplatform, devicemodel, state, country from " + tableName + " LIMIT 3";
    stmt2 = conn.createStatement();
    System.out.println("\nRetrieving inserted data:");

    res2 = stmt2.executeQuery(sql);

    while (res2.next()) {
      System.out.println( res2.getString(1) + "\t" + res2.getString(2) + "\t" + res2.getString(3) + "\t" + res2.getString(4) + "\t" + res2.getString(5) + "\t" + res2.getString(6));
    }

##示例 Java 项目

[Https://github.com/Blackmist/hdinsight-hive-jdbc](https://github.com/Blackmist/hdinsight-hive-jdbc)提供了一种在 HDInsight 上使用 Java 客户端查询配置单元。 按照在存储库中生成并运行示例的说明。

##下一步行动

现在，您已经学习了如何使用 JDBC 配置单元处理，使用下面的链接继续探讨其他方法以使用 Azure HDInsight。

* [将数据上传到 HDInsight](hdinsight-upload-data.md)
* [使用 HDInsight 配置单元](hdinsight-use-hive.md)
* [使用 HDInsight 的小猪](hdinsight-use-pig.md)
* [HDInsight 使用 MapReduce 作业](hdinsight-use-mapreduce.md)

测试
