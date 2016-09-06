---
ms.openlocfilehash: 9533766decd3ac99ef15d4e7a066536e033eb53e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="计算密集型 Java 虚拟机上的应用程序 |Microsoft Azure"
    description="了解如何创建运行计算密集型 Java 应用程序可以通过另一个 Java 应用程序来监视 Azure 的虚拟机。"
    services="virtual-machines"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="06/03/2015"
    ms.author="robmcm"/>

# 如何运行计算密集型任务以 Java 虚拟机上

使用 Azure，可以使用虚拟机来处理计算密集型任务。 例如，虚拟机可以处理任务，并将结果传递到客户端计算机或移动应用程序。 阅读这篇文章之后, 您必须了解如何创建运行计算密集型 Java 应用程序可以监视另一个 Java 应用程序的虚拟机。

本教程假定您知道如何创建 Java 控制台应用程序，可以将库导入到 Java 应用程序，并可以生成 Java 归档文件 (JAR)。 不知道 Microsoft Azure 的假定。

您将了解︰

* 如何创建虚拟机时使用 Java 开发工具包 (JDK) 已经安装。
* 如何远程登录到您的虚拟机。
* 如何创建服务总线命名空间。
* 如何创建一个 Java 应用程序，执行计算密集型任务。
* 如何创建一个 Java 应用程序监视计算密集型任务的进度。
* 如何运行 Java 应用程序。
* 如何停止 Java 应用程序。

本教程将使用计算密集型任务的旅行商问题。 下面是运行计算密集型任务的 Java 应用程序的一个示例。

![旅行商问题求解][solver_output]

以下是 Java 应用程序监视计算密集型任务的示例。

![旅行商问题的客户端][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## 若要创建虚拟机

1. 登录到[Azure 的管理门户](https://manage.windowsazure.com)。
2. 单击**新建****计算**单击**虚拟机**，和，然后单击**剪辑库中**。
3. 在**虚拟机映像选择**对话框中，选择**JDK 7 Windows Server 2012**。
请注意， **JDK 6 Windows Server 2012**使用以防您有没有准备好运行 JDK 7 中的旧版应用程序。
4. 单击**下一步**。
4. 在**虚拟机配置**对话框中︰
    1. 指定虚拟机的名称。
    2. 指定要为该虚拟机使用的大小。
    3. 在**用户名**字段中输入管理员的名称。 请记住此名称，并且您将在下一步输入的密码，您可以使用它们在您远程登录到虚拟机。
    4. 在**新密码**字段中，输入密码，在**确认**字段中重新输入。 这是管理员帐户密码。
    5. 单击**下一步**。
5. 在下一步的**虚拟机配置**对话框中︰
    1. **云服务**，使用默认设置**创建新的云服务**。
    2. Cloudapp.net**云服务 DNS 名称**的值必须唯一。 如果需要请修改此值，所以该 Azure 指示它是唯一的。
    2. 指定某个地区、 相似性组或虚拟网络。 出于本教程的需要，请指定地区如**西部美国**。
    2. 对于**存储帐户**，请选中**使用自动生成的存储帐户**。
    3. 对于**可用性设置**，请选择**（无）**。
    4. 单击**下一步**。
5. 最终在**虚拟机配置**对话框中︰
    1. 接受默认终结点项。
    2. 单击**完成**。

## 远程登录到您的虚拟机

1. 登录到[管理门户](https://manage.windowsazure.com)。
2. 单击**虚拟机**。
3. 单击想要登录到虚拟机的名称。
4. 单击**连接**。
5. 对提示作出响应，根据需要连接到虚拟机。 当系统提示您输入管理员名称和密码，使用时创建虚拟机所提供的值。

请注意 Azure 服务总线功能要求安装的 JRE 的**cacerts**存储一部分的巴尔的摩 CyberTrust 根证书。 此证书将自动包含在 Java 运行时环境 (JRE) 使用本教程。 如果 JRE **cacerts**存储区中没有该证书，请参阅有关信息的[添加到 Java CA 证书存储区的证书][add_ca_cert]添加它 （以及在 cacerts 存储查看证书信息）。

## 如何创建服务总线命名空间

要开始使用在 Azure 服务总线队列，必须先创建一个服务的命名空间。 服务命名空间用于解决应用程序中的服务总线资源提供作用域容器。

若要创建服务命名空间︰

1.  登录到[Azure 的管理门户](https://manage.windowsazure.com)。
2.  在管理门户的左导航窗格中，单击**服务总线、 访问控制和缓存**。
3.  在管理门户的左上窗格中，单击**服务总线**节点，，然后单击**新建**按钮。  
    ![服务总线节点屏幕抓图][svc_bus_node]
4.  在**创建新的服务 Namespace**对话框中，输入**Namespace**，然后以确保它是唯一的请单击**查看可用性**按钮。  
    ![创建新 Namespace 屏幕抓图][create_namespace]
5.  确保命名空间名称是可用后，选择的国家或地区的名称空间应进行托管，然后单击**创建 Namespace**按钮。  

    您所创建的名称空间将显示在管理门户，花费一点时间来激活。 等待，直到在继续下一步之前处于**活动**状态。

## 获取命名空间的默认管理凭据

为了执行管理操作，如创建新的命名空间，在队列中，您需要获取命名空间的管理凭据。

1.  在左侧的导航窗格中，单击**服务总线**节点以显示可用的命名空间的列表。
    ![可用的命名空间的屏幕抓图][avail_namespaces]
2.  选择您刚创建从显示的列表的名称空间。
    ![Namespace 列表屏幕抓图][namespace_list]
3.  右侧的**属性**窗格中列出新的命名空间的属性。
    ![属性窗格中的屏幕快照][properties_pane]
4.  **默认的密钥**将被隐藏。 单击**视图**按钮以显示的安全凭据。
    ![默认密钥屏幕抓图][default_key]
5.  记下**默认颁发者**和**默认键**将使用下面这些信息执行操作与命名空间。

## 如何创建一个 Java 应用程序，执行计算密集型任务

1. 在您的开发计算机 （它不具有将您创建的虚拟机），下载[Java 的 Azure SDK](http://azure.microsoft.com/develop/java/)。
2. 创建一个 Java 控制台应用程序，在这一节结尾处使用的示例代码。 在本教程中，我们将使用**TSPSolver.java**作为 Java 文件的名称。 修改**您\_服务\_总线\_命名空间**，**您\_服务\_总线\_所有者**，和**您\_服务\_总线\_键**占位符以使用您的服务总线**命名空间**，**默认的颁发者**和**默认关键字**值，分别。
3. 之后编写代码，导出为可运行的 Java 归档文件 (JAR) 的应用程序和到生成 JAR 包所需的库。 在本教程中，我们将使用**TSPSolver.jar**作为生成的 JAR 名称。

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## 如何创建一个 Java 应用程序监视计算密集型任务的进度

1. 在您的开发计算机上创建一个 Java 控制台应用程序，在这一节结尾处使用的示例代码。 在本教程中，我们将使用**TSPClient.java**作为 Java 文件的名称。 如上文所示，修改**您\_服务\_总线\_命名空间**，**您\_服务\_总线\_所有者**，和**您\_服务\_总线\_键**占位符以使用您的服务总线**命名空间**，**默认的颁发者**和**默认关键字**值，分别。
2. 导出应用程序可运行的 JAR，并到生成 JAR 包所需的库。 在本教程中，我们将使用**TSPClient.jar**作为生成的 JAR 名称。

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## 如何运行 Java 应用程序
运行计算密集型应用程序，首先要创建队列，然后把问题解决了旅行 Saleseman，它会将当前最佳的路由添加到服务总线队列。 计算密集型应用程序时运行 （或以后），运行客户机以显示服务总线队列中的结果。

### 若要运行计算密集型应用程序

1. 登录到您的虚拟机。
2. 创建一个文件夹，将运行您的应用程序的位置。 例如， **c:\TSP**。
3. 将**TSPSolver.jar**复制到**c:\TSP**，
4. 创建名为**c:\TSP\cities.txt** ，具有下列内容的文件。

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. 在命令提示符处，将目录更改为 c:\TSP。
6. 确保在 PATH 环境变量中的 JRE 的 bin 文件夹。
7. 您还需要创建服务总线队列之前运行 TSP 规划求解的转变。 运行以下命令以创建服务总线队列。

        java -jar TSPSolver.jar createqueue

8. 现在，创建队列时，可以运行 TSP 规划求解的转变。 例如，运行以下命令来运行 8 城市规划求解。

        java -jar TSPSolver.jar 8

 如果您不指定一个数字，它将运行 10 个城市。 规划求解找到当前最短路由，它会将其添加到队列。

> [AZURE.NOTE]
> 数值越大，您指定的时间越长规划求解将运行。 例如，运行 14 城市可能需要几分钟，并为 15 城市的运行可能需要几个小时。 增加到 16 个或更多的城市可能会导致运行时 （最终数周、 数月和数年） 的天数。 这是因为作为城市增加的数量计算规划求解的排列数的快速增长。

### 如何运行监视客户端应用程序
1. 登录到您的计算机将运行客户端应用程序的位置上。 这不需要为同一台机器运行**TSPSolver**应用程序，尽管它可以。
2. 创建一个文件夹，将运行您的应用程序的位置。 例如， **c:\TSP**。
3. 将**TSPClient.jar**复制到**c:\TSP**，
4. 确保在 PATH 环境变量中的 JRE 的 bin 文件夹。
5. 在命令提示符处，将目录更改为 c:\TSP。
6. 运行以下命令。

        java -jar TSPClient.jar

    （可选） 指定睡眠之间检查队列，通过一个命令行参数传入的分钟的数。 检查队列的默认睡眠期间为 3 分钟，可如果没有命令行参数传递给**TSPClient**。 如果您想要使用睡眠间隔不同的值，例如，1 分钟后，运行以下命令。

        java -jar TSPClient.jar 1

    客户端将运行直到看到队列消息的"完成"。 请注意，是否您运行多次出现的规划求解不运行客户端的情况下，您可能需要运行多次完全清空队列的客户端。 或者，可以删除队列并重新创建它。 若要删除队列，请运行下面的**TSPSolver** (不是**TSPClient**) 命令。

        java -jar TSPSolver.jar deletequeue

    规划求解将运行直到完成检查所有工艺路线。

## 如何停止 Java 应用程序
规划求解和客户端应用程序，您可以按**Ctrl + C**键退出如果想正常完成之前结束。


[solver_output]: ./media/virtual-machines-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md
