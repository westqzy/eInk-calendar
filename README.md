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

如果你不关心软件部分，可以直接跳到[如何使用/装配](#4-如何使用装配)小节

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

## 3. 功能改进与更新

本小节中，我将介绍针对原项目[eInkCalendarOfToxicSoul](https://github.com/breakstring/eInkCalendarOfToxicSoul)进行了哪些具体的改进和优化，包括对现有功能的改进，问题的修复等。

### 3.1. ESP—TOUCH接入网络

对于ESP32的主流方式此处列举两种：

 - Smartconfig配网：ESP32处于混杂模式下，监听网络中的所有报文，手机APP将当前连接的ssid和password编码到UDP报文中，通过广播或者组播的方式发送报文，ESP32接收到UDP报文后解码，得到ssid和password，然后使用该组ssid和password去连接网络。
 - Airkiss配网：微信硬件平台提供的一种WIFI设备快速入网配置技术，要使用微信客户端的方式配置设备入网，需要设备支持AirKiss技术。Aiskiss的原理和smartconfig很类似，设备工作在混杂模式下，微信客户端发送包含ssid和password的广播包，设备收到广播包解码得到ssid和password

此处放弃了原项目中的微信扫码(Airkiss)配置上网方案(多次尝试均无法成功，怀疑是不再支持广播)。而乐鑫开发的 ESP-TOUCH 协议可以无缝配置 Wi-Fi 设备来连接路由器。用户只需在手机上进行简单操作即可实现智能配置，因而此处仅推荐使用ESP-TOUCH APP(支持Android或IOS)并使用组播的方式进行配网。

### 3.2. 优化文字/图片位置

由于加了外壳，且部分显示参数长度并不统一(如天气有：多云、阴等；空气质量等级有：良、轻度污染等)，有时候会出现文字被遮挡等显示不全的情况。此处对部分文字/图片的位置和排布进行了优化。

### 3.3. fillScreen替换clearScreen

针对`display.clearScreen` 报warning的问题，将`clearScreen`函数用`fillScreen`函数替换

```cpp
//display.clearScreen(GxEPD_WHITE);
display.fillScreen(GxEPD_WHITE);
```

### 3.4. 重新整合天气图标

和风天气提供了一套漂亮的[天气图标与天气图标字体](https://dev.qweather.com/docs/resource/icons/)，使用SVG格式，方便嵌入到网站或APP中。

其前身是[天气图标项目](https://github.com/qwd/WeatherIcon)，该项目提供了两套PNG格式的图标，包括彩色和单色版本。
原项目的图标便取自该项目，奈何其不再提供任何更新，且与目前最新的和风天气图标代码不匹配。

鄙人曾尝试将目前的SVG图标转为bmp格式，以供墨水屏显示，结果失败了(不知什么原因，转出来的图片是空白)。退而求其次之下，对老版图标进行了重新整合，包括但不限于部分天气(如多云、少云、晴间多云等)共用图标，磕磕碰碰也算解决了该问题。

### 3.5. 解Gzip流

和风天气的Web API默认采用[Gzip](https://www.gnu.org/software/gzip/)进行压缩，这将极大的减少网络流量，加快请求。

着实被这一手坑了一把，查看串口发现返回的解析都是乱码，找了半天发现和风天气的开发服务中有以上那段话。网上[查阅资料](https://blog.csdn.net/weixin_42880082/article/details/111861897)显示，可以通过在请求中加入`&gzip=n`来返回不进行压缩的数据，但通过测试发现该方法已经失效，和风天气已经默认gzip压缩(加不加`&gzip=n`都一样，真垃圾！)。遂只能另想别的方法。

目前通过[ArduinoUZlib](https://github.com/tignioj/ArduinoUZlib)库，用来解压https返回的gzip流。相关使用可见[src/QWeather.cpp](src/QWeather.cpp)。

### 3.6. 替换ESP32分区表

`board_build.partitions`是在PlatformIO项目的[platformio.ini](platformio.ini)配置文件中用来指定ESP32分区表的设置项。

ESP32的闪存（Flash）可以被分区为多个区域，这些区域的大小和布局可以通过分区表来定义。`board_build.partitions`选项允许选择使用哪一个分区表。分区表通常是CSV文件，定义了每个分区的类型，大小，和位置。Arduino ESP32核心提供了多种预定义的分区表，可以选择其中的一个，或者根据需要自定义自己的分区表。

原项目中采用的分区表为`huge_app.csv`，该分区表会导致build Filesystem Imag时报错：`SPIFFS_write error(-10001): File system is full`。

只需要适当调整`app0`和`spiffs`的大小即可，此处`board_build.partitions`使用我们自己自定义的分区表[my.csv](my.csv)。

## 4. 如何使用/装配

### 4.1. 打开淘宝/咸鱼

买齐配件等到货

### 4.2. VSCode和PlatformIO IDE的安装

不多赘述，网上教程很多，随便找一个即可，如[这个](https://blog.csdn.net/qq_40018676/article/details/128680677)。

### 4.3. clone这个库并烧写程序

- clone本库到本地，**记得修改和风天气API的Key**！具体在[config.h](src\config.h)里：
```cpp
const String QWEATHER_API_KEY = "********************";
```

- 连接电子墨水屏到微雪的ESP32开发板上，其实很简单，就一个软排线接口，插上口按下卡扣就好。**烧写程序时记得通过USB连接电脑**！

- 通过VSCode中PlatformIO插件中的Open Project打开本地项目(`platformio.ini`所在文件夹)，初次打开项目可能会下载必要的库，可能会很慢或失败，多半和网络原因有关

<div style="display: flex; justify-content: space-around;">
    <img src="/images/s1.png" width="400" style="flex-grow: 1;">
    <img src="/images/s2.png" width="400" style="flex-grow: 1;">
</div>


- 编译文件系统镜像并上传到驱动板

    <img src="/images/s3.png" width="400">

- 编译项目并上传到驱动板
 
    <img src="/images/s4.png" width="400">

### 4.4. 配网

- 初次联网(初次烧录)或者更换网络环境需要进行配网操作，此处使用ESP-TOUCH APP(支持Android或IOS)并使用组播的方式进行配网。

- ESP32如果无法连接网络，会在墨水屏上显示：

    <img src="/images/ESPconfig.jpg" width="400">

- 此时在手机端打开Esptouch APP，**选择 EspTouch**(不是 EspTouch V2)，并按如下图所示配置WIFI名称、WIFI密码，并**勾选组播选项**，即可让ESP32连接手机接入的无线网络之中：
    
    <img src="/images/ESPtouch.jpg" width="400">

- 不同系统下的APKs：

IOS系统下：[ESP-TOUCH for iOS](https://apps.apple.com/cn/app/espressif-esptouch/id1071176700)

Android系统下：[ESP-TOUCH for Android](https://github.com/EspressifApp/EsptouchForAndroid/releases/tag/v2.0.0/esptouch-v2.0.0.apk)

### 4.5. 安装

按照自己的喜好，把器件都安装进去即可！以下是我的方案。

<img src="/images/z1.jpg" width="400">

<div style="display: flex; justify-content: space-around;">
    <img src="/images/z2.jpg" width="400" style="flex-grow: 1;">
    <img src="/images/z3.jpg" width="400" style="flex-grow: 1;">
</div>