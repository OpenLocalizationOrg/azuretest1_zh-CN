---
ms.openlocfilehash: fd7930ffe83ffe6b330cc5b1c98503df4cca5a33
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
每小时基于陆地的气象观测从 NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">合并数据从 201304 到 201310</a>)。<p> </p>天气数据涵盖从机场气象站，涵盖的时间段 4 月-10 月 2013年进行观测。 之前上传到 Azure ML Studio，数据集是按下面这样处理︰<ul><li>气象站 Id 映射到相应的机场 Id</li><li>不与 70 繁忙机场的气象站都被筛选掉</li><li>日期列分割成单独的年、 月和日列</li><li>选择以下各列︰ AirportID、 年、 月、 日、 时间、 时区、 SkyCondition、 可见性、 WeatherType、 DryBulbFarenheit、 DryBulbCelsius、 WetBulbFarenheit、 WetBulbCelsius、 DewPointFarenheit、 DewPointCelsius、 RelativeHumidity、 WindSpeed、 WindDirection、 ValueForWindCharacter、 StationPressure、 PressureTendency、 PressureChange、 SeaLevelPressure、 RecordType、 HourlyPrecip、 Altimeter</li></ul>