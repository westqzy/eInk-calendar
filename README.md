## 1. 说明

这个项目是基于 breakstring's [eInkCalendarOfToxicSoul](https://github.com/breakstring/eInkCalendarOfToxicSoul) 进行的二次开发。我从这个原始项目中获得了很多启示，然而，由于原项目已经有些年份，我注意到一些功能不能正常运行。因此我在原项目基础上进行了一些改进和扩展，以确保所有的功能都能正常工作。我非常感谢原作者 breakstring 的辛勤努力和开放分享。他的贡献为我提供了一个很好的参考，让我有机会学习和改进。

## 2. 方案

### 2.1. 硬件

- **屏幕**：来自微雪的[5.83inch e-Paper HAT Manual](https://www.waveshare.net/wiki/5.83inch_e-Paper_HAT_Manual)，分辨率648 × 480，外形尺寸125.40mm × 99.50mm × 1.18mm，我选的黑白款，裸屏淘宝价格为231元。
  
<img src="/images/image-3.png" width="400">


- **计算单元**：为了方便同样选自来自微雪的[E-Paper ESP32 Driver Board](https://www.waveshare.net/wiki/E-Paper_ESP32_Driver_Board)，这是一款电子墨水屏无线网络驱动板，板载 ESP32，支持Arduino开发。淘宝价格78元。
<img src="/images/image-1.png" width="300">
<img src="/images/image-2.png" width="300">

- **电源**：可以通过电脑上的USB接口用micro USB线直接通过开发板来进行供电和调试工作。如果后期程序调试完成希望封装成一个整体，可以外接一个电源。此处为了省事，直接在淘宝选择了中顺动力的电池，定制充放电接口：充电口type-c母，放电口microUSB公(可选带电量板)。具体型号比较合适的有：5V恒压5000mAh 105 x 60 x 6mm、5V恒压4000mAh 85 x 63 x 7mm等。电源的成本可以控制在60元以内。
<div style="display: flex; justify-content: space-around;">
    <img src="/images/b1.jpg" width="400" style="flex-grow: 1;">
    <img src="/images/b2.jpg" width="400" style="flex-grow: 1;">
</div>

- **外壳**：3D打印、买个相框塞把模块塞进去都是不错的选择。此处我选择在万能的淘宝买了1500颗8mm散装小颗粒积木，大约20块钱。
<img src="//images/jimu.jpg" width="400">
<img src="/images/ke.jpg" width="400">


### 2.2. 软件