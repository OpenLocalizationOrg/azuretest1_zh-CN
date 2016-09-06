---
ms.openlocfilehash: 0ef0e99cff37068a9ec91f44b83cf1e55bfdb1e9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 配置单元 HDInsight 表中的数据采样 |Microsoft Azure"
    description="向下采样 Azure HDInsight 配置单元表中的数据"
    services="machine-learning,hdinsight"
    solutions=""
    documentationCenter=""
    authors="hangzh-msft"
    manager="paulettm" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2015"
    ms.author="hangzh;bradsev" />

# 在 Azure 配置单元 HDInsight 表中的示例数据

如果您要分析的数据集比较大，通常是一个好办法下双样本的数据，以将其减小到一个较小但具有代表性和更易于管理的大小。 这简化了数据的了解、 研究，以及功能的工程。 高级分析过程和技术 （调整） 的 Azure 机器学习，其作用是使数据处理功能和机器学习模型的快速原型开发。

在本文中，我们将介绍如何关闭示例使用查询配置单元的配置单元 Azure HDInsight 表中的数据。 我们将介绍三种得到普遍使用的采样方法︰ 

* 统一的随机抽样 
* 按组随机抽样 
* stratified 的采样

您应该提交 Hadoop 群集的头节点上的 Hadoop 命令行控制台配置单元查询。 若要执行此操作，登录到 Hadoop 群集的头节点，打开 Hadoop 命令行控制台，提交配置单元查询，从那里。 有关提交 Hadoop 命令行控制台中的配置单元查询的说明，请参阅[如何提交配置单元查询](machine-learning-data-science-process-hive-tables.md#submit)。

## <a name="uniform"></a> 统一的随机抽样 ##
统一的随机抽样意味着数据集中的每一行都有同等的被抽样的机会。 这可以通过添加额外的字段 rand （） 到数据集在内部的"选择"查询中，并在外部的"选择"查询中该条件在该随机的字段上。

下面是一个示例查询︰

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

在这里，`<sample rate, 0-1>`指定的用户想要取样的记录的比例。

## <a name="group"></a> 按组随机抽样 ##

当采样的分类数据，您可以包括或排除某一分类变量的某个特定值的实例的所有。 这是通过"按组采样"的含义。
例如，如果您有某一分类变量"状态"，其值的纽约州、 马萨诸塞州、 CA、 新泽西、 宾夕法尼亚州等，所需记录的相同的状态将始终在一起，它们抽样或不。

下面是一个示例查询该示例按组︰

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Stratified 的采样

随机抽样被 stratified 关于某一分类变量获取样本具有的值时，分类，都在如父的人口从中获取示例中所示相同的比率。 上面，使用相同的示例假设您的数据具有状态的子群体，新泽西有 100 的观察结果，纽约州有 60 观测值，并且华盛顿 300 观察说。 如果指定 stratified 为 0.5 的采样速率，然后获得该示例应有大约 50、 30 和新泽西州、 纽约州，以及华盛顿州 150 观测值分别。

下面是一个示例查询︰

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


有关更高级的采样方法配置单元中的可用信息，请参阅[LanguageManual 采样](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling)。
 
测试
