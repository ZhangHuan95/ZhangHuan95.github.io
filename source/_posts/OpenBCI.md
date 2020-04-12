---
title: OpenBCI
typora-root-url: ..
date: 2020-04-09 22:59:58
tags: OpenBCI
comments: true
---
# 基于头部信号的小车运动控制实现 
## 前言
此项目为本人18年的本科毕业设计，最近刚好开始写博客，所以就这个翻出来和大家分享一下。

## 废话不多说先看效果

{% raw %}
<div style="position: relative; width: 100%; height: 0; padding-bottom: 75%;"><iframe src="//player.bilibili.com/player.html?aid=412655697&bvid=BV1xV411o73Y&cid=177388746&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" style="position: absolute; width: 100%; height: 100%; left: 0; top: 0;"></iframe></div>
{% endraw %}

## 前期准备
### 硬件
1. OpenBCI相关设备

   > OpenBCI是个面向脑机接口EEG信号采集的开源硬件。OpenBCI Cyton是一个Arduino兼容的带有8通道神经接口和一个32位处理器开发板。
   >
   > ![board](/images/OpenBCI/openbci_board.png)
   >
   > <img src="/images/OpenBCI/usb_dongle.png" alt="usb_dongle" style="zoom:50%;" />


2. 基于Arduino单片机的可编程小车

   > ![car](/images/OpenBCI/car.png)
   >
   > 产品所用元器件： WIFI模块\*1，arduino uno r3单片开发板\*1，arduino 拓展版\*1，直流减速电机\*4，轮胎\*4，电池盒\*1，18650Li-ino电池\*2,电压显示器\*1，底盘\*1。
### 软件
1. OpenBCI_GUI
2. OpenBCI_Python
3. Arduino 软件（IDE）
### 一、理论知识

1. 脑机接口

   > 脑机接口从定义上说就是如何使用头部信号与外部机械进行直接交互的一项技术，并且对脑机接口的相关研究已经超过40年了。从上个世纪以来，人们不断地从各类试验之中总结经验，对此相关的知识逐渐积累，并且以多年在动物身上的试验结果中总结的方法，把一些相关的植入性设备进行人体植入，主要用于恢复已经受伤的感觉器官，如眼睛，耳朵和运动肢体等。
   >
   > ![bci](/images/OpenBCI/bci.png)

2. 脑电采集设备

   > 1. 侵入式
   >
   >    > 此类接口通常是将电极直接接入到大脑的灰质上,因为减少了传播介质的干扰,由此得到的神经信号质量较好。但是,由于植入电极须通过手术的方式,导致其本身会伴随较大风险。
   >    >
   >    > ![](/images/OpenBCI/device1.png)
   >
   > 2. 半侵入式
   >
   >    > 此类接口的位置一般是在大脑灰质之外，颅腔之内，虽然它的神经信号质量不如侵入式脑机接口，可是还是要比非侵入式要好。
   >    >
   >    > ![](/images/OpenBCI/device2.png)
   >
   > 3. 非侵入式
   >
   >    > 此类接口由于不需要植入大脑内部，所以不会对头部造成伤害，许多的此类设备直接与头部皮层相接，就像一顶帽子一样戴在头上既可。
   >    >
   >    > ![](/images/OpenBCI/device3.png)

### 二、研究方法与思路

#### (一)系统设计

1. 连接

   系统启动时，上位机与下位机通过WiFi建立通信连接，采集设备通过蓝牙建立通信连接。

2. 传输

   USB Dongle接收实时脑电信号，将其传输到电脑上。

3. 处理

   PC端将接收到的实时数据，进行处理工作。

4. 识别

   系统将处理后的数据，进行信号识别工作，并将结果传输给下位机。

5. 控制

   下位机接收上位机的控制指令后，对小车运动状态进行实时控制。
   
   ![](/images/OpenBCI/Snipaste_2020-04-12_18-27-25.png)

#### (二)机器小车

1. 连接

   下位机与上位机之间通过WiFi建立通信连接。

2. 启动

   通过串口读取来自上位机的控制指令，并且将第一个左转信号视为启动信号。


3. 行进

   启动后，小车向前行进，并且实时检测串口信号。

4. 控制

   根据控制命令，控制小车左转或右转后，继续行进。
   
   ![](/images/OpenBCI/Snipaste_2020-04-12_18-27-56.png)
#### (三)信号预处理

1. 时间片数据

   将每秒钟的250个数据封装成一段。

2. 去基线漂移

   数字信号中会含有基线干扰信号（低频噪音），会对信号分析产生不利影响。需要通过预处理消除信号基线


3. 陷波滤波

   在我国采用的是50 Hz频率的交流电，所以在平时需要对信号进行采集处理和分析时，常会存在50 Hz的工频干扰。

4. 带通滤波

   经过多次试验发现，选取在7-13Hz范围的波形信号最为明显。

   ![](/images/OpenBCI/Snipaste_2020-04-12_18-28-04.png)

#### (四)信号识别

1. 保持睁眼时

![](/images/OpenBCI/figure1.png)

2. 单次眨眼

   ![](/images/OpenBCI/Snipaste_2020-04-12_18-33-49.png)

3. 两次眨眼

   ![](/images/OpenBCI/Snipaste_2020-04-12_18-33-56.png)



### 三、测试

#### （一）机器小车测试

待续~~~

#### （二）信号处理系统测试

待续~~~

#### （三）系统集成测试

![](/images/OpenBCI/Snipaste_2020-04-12_18-39-27.png)

![](/images/OpenBCI/Snipaste_2020-04-12_18-39-36.png)



### 四、建议及总结

1. #### 改进方法，减少系统误差，减少控制时延。
2. #### 开源硬件成本低廉，开发方便。
3. #### 该系统适用性强，经过适当改造可以控制其他设备。
4. #### 对于特定人群例如残疾人、聋哑人的应用前景更为广阔。

## 未完待续！不定期炸更。