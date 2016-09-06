---
ms.openlocfilehash: 5a83c6741b5209d2e1749c1fef1a6ed6feb432ee
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
资源|免费|共享 （预览）|基本|标准|高级 （预览）</th>
---|---|---|---|---|---
[Web、 移动设备、 或 API 应用程序](../services/app-service/)每个[应用程序服务计划](web-sites-web-hosting-plan-overview.md)<sup>1</sup>|10|100|无限制<sup>2</sup>|无限制<sup>2</sup>|无限制<sup>2</sup>
每个[应用程序服务计划](web-sites-web-hosting-plan-overview.md)的[逻辑应用程序](../services/app-service/)</a><sup>1</sup>|10|10|10|每个内核的 20|每个内核的 20 
[应用程序服务计划](web-sites-web-hosting-plan-overview.md)|每个地区的 1|每个资源组 10|每个资源组 10|每个资源组 10|每个资源组 10
计算实例类型|共享|共享|专用的<sup>3</sup>|专用的<sup>3</sup>|专用的<sup>3</sup></p>
[向外扩展](web-sites-scale.md)（最大值的实例）|1 共享|1 共享|专用的 3<sup>3</sup>|专用的 10<sup>3</sup>|专用的 50<sup>3、 4</sup>
存储<sup>5</sup>|1 GB<sup>5</sup>|1 GB<sup>5</sup>|10 GB<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
CPU 时间 （天）<sup>6</sup>|60 分钟|240 分钟|不受限制，按标准[费率](../pricing/details/app-service/)的付薪</a>|不受限制，以标准的比率的付薪|不受限制，以标准的比率的付薪
内存 （1 小时）|每个应用程序服务计划 1024 MB|每个应用程序的 1024 MB|N/A|N/A|N/A
Bandwidth|165 MB|不受限制，[数据传输速率](../pricing/details/data-transfers/)应用|不受限制，数据传输速率应用|不受限制，数据传输速率应用|不受限制，数据传输速率应用
应用程序体系结构|32 位|32 位|32 位 64 位|32 位 64 位|32 位 64 位
每个实例的 web 套接字<sup>7</sup>|5|35|350|无限|无限
每个应用程序并发[调试程序连接](web-sites-dotnet-troubleshoot-visual-studio.md)|1|1|1|5|5
[azurewebsites.net 与 FTP/S 和 SSL 的子域](web-sites-configure-ssl-certificate.md)|X|X|X|X|X
[自定义的域](web-sites-custom-domain-name.md)支持||X|X|X|X
[SSL 支持](web-sites-configure-ssl-certificate.md)自定义的域|||无限|无限制、 5 SNI SSL 和 1 包括的 IP SSL 连接|无限制、 5 SNI SSL 和 1 包括的 IP SSL 连接
集成的负载平衡器||X|X|X|X
[始终打开](web-sites-configure.md)|||X|X|X
[定时的备份](web-sites-backup.md)||||每天一次|每隔 5 分钟记录一次<sup>8</sup>
[自动缩放](web-sites-scale.md)|||X|X|X
[WebJobs](web-sites-create-web-jobs.md)<sup>9</sup>|X|X|X|X|X
[Azure 计划](../services/scheduler/)支持||X|X|X|X
[终结点监视](web-sites-monitor.md)|||X|X|X
[暂存槽 （预览）](web-sites-staged-publishing.md)||||5|20
自定义每个应用程序域</a>||500|500|500|500
SLA||<p>|99.9%|99.95%<sup>10</sup>|99.95%<sup>10</sup>

<sup>1</sup>应用程序和存储配额是每个应用程序服务计划除非另外说明。  
<sup>2</sup>可以承载在这些计算机的应用程序的实际数量取决于应用程序的活动、 机实例和相应的资源利用率的大小。  
<sup>3</sup>专用的实例可以是不同的大小。 有关详细信息，请参阅[应用程序服务的定价](/pricing/details/app-service/)。 其他实例均可通过打开支持请求。  
<sup>4</sup>高级层允许最多到 50 计算实例 （如有提供） 和 500 GB 的磁盘空间时使用的应用程序服务的环境中，并将 20 否则计算实例和 250 GB 存储。  
<sup>5</sup>存储限制是跨所有应用程序在同一应用程序服务计划的内容的总量。 可以通过打开支持请求增加存储限制。  
<sup>6</sup>这些资源受到物理资源上的专用实例 （实例的大小和数量的实例）。  
<sup>7</sup>基本层对两个实例中缩放应用程序时，如果您有 350 并发连接的两个实例的每个。  
<sup>8</sup>特优层使用服务的应用程序环境，和 50 次每日以其他方式允许最多每隔 5 分钟向下的备份间隔。  
<sup>9</sup>根据需要，按计划，运行自定义可执行文件和/或脚本或持续作为后台任务在您的应用程序的服务实例。 始终是连续的 WebJobs 执行必需的。 Azure 计划程序可用或标准时需要计划 WebJobs。  
<sup>10</sup>的部署使用多个实例使用 Azure 流量管理器配置为故障转移提供了 99.95%的 SLA。  
