## 1. 说明

这个项目是基于 breakstring's [eInkCalendarOfToxicSoul](https://github.com/breakstring/eInkCalendarOfToxicSoul) 进行的二次开发。我从这个原始项目中获得了很多启示，然而，由于原项目已经有些年份，我注意到一些功能不能正常运行。因此我在原项目基础上进行了一些改进和扩展，以确保所有的功能都能正常工作。我非常感谢原作者 breakstring 的辛勤努力和开放分享。他的贡献为我提供了一个很好的参考，让我有机会学习和改进。

## 2. 方案

### 2.1. 硬件

- **屏幕**：来自微雪的[5.83inch e-Paper HAT Manual](https://www.waveshare.net/wiki/5.83inch_e-Paper_HAT_Manual)，分辨率648 × 480，外形尺寸125.40mm × 99.50mm × 1.18mm，我选的黑白款，裸屏淘宝价格为231元。
  
<img src="/images/image-3.png" width="600">


- **计算单元**：为了方便同样选自来自微雪的[E-Paper ESP32 Driver Board](https://www.waveshare.net/wiki/E-Paper_ESP32_Driver_Board)，这是一款电子墨水屏无线网络驱动板，板载 ESP32，支持Arduino开发。淘宝价格78元。

<div style="display: flex; justify-content: space-around;">
    <img src="/images/image-1.png" width="400" style="flex-grow: 1;">
    <img src="/images/image-2.png" width="400" style="flex-grow: 1;">
</div>

- **电源**：可以通过电脑上的USB接口用micro USB线直接通过开发板来进行供电和调试工作。如果后期程序调试完成希望封装成一个整体，可以外接一个电源。此处为了省事，直接在淘宝选择了中顺动力的电池，定制充放电接口：充电口type-c母，放电口microUSB公(可选带电量板)。具体型号比较合适的有：5V恒压5000mAh 105 x 60 x 6mm、5V恒压4000mAh 85 x 63 x 7mm等。电源的成本可以控制在60元以内。
 
<div style="display: flex; justify-content: space-around;">
    <img src="/images/b1.jpg" width="400" style="flex-grow: 1;">
    <img src="/images/b2.jpg" width="400" style="flex-grow: 1;">
</div>

- **外壳**：3D打印、买个相框塞把模块塞进去都是不错的选择。此处我选择在万能的淘宝买了1500颗8mm散装小颗粒积木，大约20块钱。
<img src="/images/jimu.jpg" width="400">
<img src="/images/ke.jpg" width="400">


### 2.2. 软件

- **框架**：ESP32是一款由Espressif Systems开发的低功耗Wi-Fi和蓝牙/蓝牙LE双模芯片。它由一个双核处理器、众多I/O引脚和丰富的外设接口组成，适用于各种物联网(IoT)和嵌入式应用。Arduino也提供了对ESP32的支持，可以在Arduino IDE中直接编程ESP32。使用Arduino进行ESP32开发，并利用Arduino丰富的库和社区资源，编程体验也相对简洁友好。

- **开发工具**：[Visual Studio Code](https://code.visualstudio.com/)中的插件[PlatformIO](https://platformio.org/)。PlatformIO 最大的好处主要有几点：一是多板卡和多处理器架构支持，无论是使用 Arduino、ESP8266 还是其他的开发板，都可以在 PlatformIO 下进行编程。二是包含了强大的库管理功能，方便开发者搜索、安装和更新库。三是提供了一站式的开发解决方案，包括代码编写、编译、烧写、调试、串口监测等功能。四是完美地融入了 VS Code。

- **相关类库**: 主要用到的第三方库可参见原项目[eInkCalendarOfToxicSoul](https://github.com/breakstring/eInkCalendarOfToxicSoul)，除此以外添加了用于解压gzip流的方案
  
    - [Adafruit GFX](https://github.com/adafruit/Adafruit-GFX-Library): 由著名的电子硬件社区Adafruit提供的一套图形图像引擎。做各种需要显示输出的Arduino应用一般都少不了它
    - [GxEPD2](https://github.com/ZinggJM/GxEPD2):基于[Adafruit_GFX](https://github.com/adafruit/Adafruit-GFX-Library)库来驱动各种电子墨水屏
    - [U8g2 for Adafruit GFX](https://github.com/olikraus/U8g2_for_Adafruit_GFX)：一套基于[U8g2](https://github.com/olikraus/U8g2)字体引擎来通过Adafruit GFX来显示文字的第三方库
    - [u8g2_fontmaker](https://github.com/breakstring/u8g2_fontmaker) 配合U8g2 for Adafruit GFX以支持中文字库
    - [ArduinoJSON](https://arduinojson.org/):用于处理JSON字符串
    - [ArduinoUZlib](https://github.com/tignioj/ArduinoUZlib):Arduino uzlib库，用来解压https返回的gzip流，内存占用较小

- **相关服务**：具体可参见原项目[eInkCalendarOfToxicSoul](https://github.com/breakstring/eInkCalendarOfToxicSoul)，此处简要列举
  
  - 毒鸡汤：为了确保服务的长期稳定，原作者并未采用网络上现有的API来处理，而是直接硬编码到了[src/toxicsoul.h](src/toxicsoul.h)里。
  - [IP地址查询](https://www.myip.la/)：用来通过当前设备的IP地址查询得知当前位置。具体可见 [src/MyIP.h](src/MyIP.h) 和 [src/MyIP.cpp](src/MyIP.cpp)
  - 字体：项目中的字体使用了[造字工房](https://www.makefont.com/)的部分非商用字体来生成。
  - 天气服务：此处使用[和风天气开发平台](https://dev.qweather.com/)的服务。所以需要注册账号并获取到自己的一个应用程序Key来替换[src/config.h](src/config.h)中的占位符。具体相关代码可以参见 [src/QWeather.h](src/QWeather.h) 和 [src/QWeather.cpp](src/QWeather.cpp)
```cpp
const String QWEATHER_API_KEY = "********************";
```

