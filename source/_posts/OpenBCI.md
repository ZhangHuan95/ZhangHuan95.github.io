---
title: OpenBCI
typora-root-url: ..
date: 2020-04-09 22:59:58
tags: OpenBCI
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
### 理论知识

1. 脑机接口

   > 脑机接口从定义上说就是如何使用头部信号与外部机械进行直接交互的一项技术，并且对脑机接口的相关研究已经超过40年了。从上个世纪以来，人们不断地从各类试验之中总结经验，对此相关的知识逐渐积累，并且以多年在动物身上的试验结果中总结的方法，把一些相关的植入性设备进行人体植入，主要用于恢复已经受伤的感觉器官，如眼睛，耳朵和运动肢体等。
   >
   > ![bci](/images/OpenBCI/bci.png)