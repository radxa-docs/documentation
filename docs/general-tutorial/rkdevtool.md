---
sidebar_label: 'Rockchip开发工具'
sidebar_position: 2
---

# 简介

Rockchip 在 Windows 平台上提供了 [RKDevTool](#RKDevTool) 工具作为基础开发辅助工具， 而在 Linux 和 Mac 平台下，则需要使用 [rkdeveloptool](#rkdeveloptool) 来进行简单开发。  

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
<TabItem value="RKDevtool" label="Windows：RKDevtool" default>

## RKDevTool

目前瑞莎支持使用 RKDevTool 来对进入 Maskrom 模式的设备进行基础开发操作， 
其中最常用的便是烧写 BootLoader 以及烧写系统(该方式常被称为：线刷)。

### Windows 安装 RKDevTool

在 Windows 下的安装非常简单，只需要下载开发工具以及 RK 驱动直接安装： 

RK 开发工具： [RKDevTool V2.96](https://dl.radxa.com/tools/windows/RKDevTool_Release_v2.96_zh.zip) (压缩包中包含中文使用文档)  
RK 官方驱动程序： [DriverAssitant V5.0](https://dl.radxa.com/tools/windows/DriverAssitant_v5.0.zip)  

使用 RK 驱动助手工具来安装驱动。 
在此过程中，不需要连接您的 Rockchip 设备，只需下载压缩包并解压 DriverAssitant V5.0.zip。 
然后双击 DriverInstall.exe，启动工具并点击 Install Driver 安装驱动。 
如果你已经为其他 Rockchip 设备安装过 Rockchip USB 驱动，请先点击 "卸载驱动"。  
![RK Driver](/img/configuration/RK-Driver-Assistant-Install-Uninstall.webp)

以下是安装完成后打开 RKDevTool 并连接进入 Maskrom 设备的示例：  
![RK Dev interface](/img/rock5a/on-maskrom.webp)

### 烧写系统镜像

1. 打开 RKDevTool  
![RKDevTool zh](/img/configuration/rkdevtool-zh.webp)

2. 将设备通过 OTG 接入主机并启动进入 Maskrom 模式  
:::caution
每个主板进入 Maskrom 模式的方式会有所区别，通用的方法为：移除所有存储介质，连接OTG线并上电，进入 Maskrom 模式后再安装存储介质，通常还需要按下 Maskrom 按键或短接 Maskrom 引脚。  
:::
![RKDevTool zh maskrom](/img/configuration/rkdevtool-zh-maskrom.webp)

3. 烧录配置  
:::caution
此处选择的镜像应该为 img 格式，下载的镜像默认为压缩包格式，需要解压缩后再进行烧录。  
:::
在 storage 选项中选择需要安装系统的介质  
![RKDevTool zh storage](/img/configuration/rkdevtool-zh-storage.webp)  
点击空白单元格配置选择 [loader](#spi-镜像及-loader-文件) 和 img 镜像文件 
![RKDevTool zh choose](/img/configuration/rkdevtool-zh-choose.webp) 

4. 勾选强制按地址写后点击执行
![RKDevTool zh flashing](/img/configuration/rkdevtool-zh-flashing.webp) 

5. 等待烧写完成，随后设备自动重启进入系统  
![RKDevTool zh complete](/img/configuration/rkdevtool-zh-complete.webp) 

### 烧写 Bootloader 到 SPI flash

在使用 NVME SSD 作为系统启动磁盘的时候，需要在 SPI flash 中烧录 Bootloader 来引导系统。  

以下是使用 RKDevTool 烧录 Bootloader 的步骤：  

1. 获取[对应平台 bootloader 和 U-Boot image](#spi-镜像及-loader-文件)  

2. 主板进入 Maskrom 模式， 选择对应的 bootloader 和 SPI image  
![RKDevTool zh SPINOR](/img/configuration/rkdevtool-zh-spinor.webp) 

3. 点击执行，等待烧写完成  
![RKDevTool zh SPI complete](/img/configuration/rkdevtool-zh-spi-complete.webp) 

</TabItem>
<TabItem value="rkdeveloptool" label="Linux/MacOS: rkdeveloptool">

## rkdeveloptool

在 Linux/MacOS 上，没有专门的软件包，需要我们从源代码来编译构建 rkdeveloptool 工具。 

### Linux/MacOS 安装 rkdeveloptool

以在 Debian 的 Linux 发行版上编译 rkdeveloptool 工具为例，请按照以下说明操作：  

1. 安装编译依赖:  

```bash
sudo apt-get install libudev-dev libusb-1.0-0-dev dh-autoreconf pkg-config libusb-1.0
```

2. 克隆源码并编译:  

```bash
 git clone https://github.com/rockchip-linux/rkdeveloptool
 cd rkdeveloptool
 autoreconf -i
 ./configure
 make
```

3. 将当前目录下的 rkdeveloptool 可执行文件添加到系统工具中。

```bash
 sudo cp rkdeveloptool /usr/local/bin/
```

#### 安装中可能遇到的问题

编译错误 1：

```bash
    configure: error: in `/home/radxa/rkdeveloptool':
    configure: error: C++ preprocessor "/lib/cpp" fails sanity check

```

该报错为缺少必要的 C++ 库，可通过以下命令安装：  

```bash
    sudo apt-get install build-essential
    sudo apt-get install g++
```

编译错误 2：  

```bash
make[1]: Entrando no diretório '/home/radxa/rkdeveloptool'
g++ -DHAVE_CONFIG_H -I. -I./cfg  -Wall -Werror -Wextra -Wreturn-type -fno-strict-aliasing -D_FILE_OFFSET_BITS=64 -D_LARGE_FILE -I/usr/include/libusb-1.0   -g -O2 -MT main.o -MD -MP -MF .deps/main.Tpo -c -o main.o main.cpp
main.cpp: In function ‘bool _Z9mergeBootv.part.0()’:
main.cpp:1493:43: error: ‘%s’ directive output may be truncated writing up to 557 bytes into a region of size 5 [-Werror=format-truncation=]
 1493 |         snprintf(buffer, sizeof(buffer), "%s", chip);
      |                                           ^~
......
 1534 |                 chipType = convertChipType(chip + 2);
      |                            ~~~~~~~~~~~~~~~~~~~~~~~~~
In file included from /usr/include/stdio.h:894,
                 from DefineHeader.h:3,
                 from main.cpp:11:
/usr/include/x86_64-linux-gnu/bits/stdio2.h:71:35: note: ‘__builtin_snprintf’ output between 1 and 558 bytes into a destination of size 5
   71 |   return __builtin___snprintf_chk (__s, __n, __USE_FORTIFY_LEVEL - 1,
      |          ~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   72 |                                    __glibc_objsize (__s), __fmt,
      |                                    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   73 |                                    __va_arg_pack ());
      |                                    ~~~~~~~~~~~~~~~~~
cc1plus: all warnings being treated as errors
make[1]: *** [Makefile:491: main.o] Erro 1
make[1]: Saindo do diretório '/home/radxa/rkdeveloptool'
make: *** [Makefile:511: all-recursive] Erro 1
```

则需要将 main.cpp 中的代码做一些修改：  

```bash
1490    static inline uint32_t convertChipType(const char* chip) {
1491	    char buffer[5];
1492	    memset(buffer, 0, sizeof(buffer));
1493	    snprintf(buffer, sizeof(buffer), "%s", chip);
1494	    return buffer[0] << 24 | buffer[1] << 16 | buffer[2] << 8 | buffer[3];
1495    }
```

其中的 1493 行修改为： 

```bash
1493        snprintf(buffer, sizeof(buffer), "%.*s", static_cast<int>(sizeof(buffer) - 1), chip);
```

再重新执行 make 命令：  

```bash
    make
```

### rkdeveloptool 的使用

在键入`rkdeveloptool`命令后，可以看到其支持的功能列表参数：

```bash
---------------------Tool Usage ---------------------
Help:                   -h or --help
Version:                -v or --version
ListDevice:             ld
DownloadBoot:           db <Loader>
UpgradeLoader:          ul <Loader>
ReadLBA:                rl  <BeginSec> <SectorLen> <File>
WriteLBA:               wl  <BeginSec> <File>
WriteLBA:               wlx  <PartitionName> <File>
WriteGPT:               gpt <gpt partition table>
WriteParameter:         prm <parameter>
PrintPartition:         ppt
EraseFlash:             ef
TestDevice:             td
ResetDevice:            rd [subcode]
ReadFlashID:            rid
ReadFlashInfo:          rfi
ReadChipInfo:           rci
ReadCapability:         rcb
PackBootLoader:         pack
UnpackBootLoader:       unpack <boot loader>
TagSPL:                 tagspl <tag> <U-Boot SPL>
-------------------------------------------------------
```

在每项功能的参数前都有其功能介绍，对于常用功能此处会有一个简单使用例子：

#### 查看已连接的 Maskrom 设备： 

```bash
rkdeveloptool ld

# output
DevNo=1 Vid=0x2207,Pid=0x350b,LocationID=106 Maskrom
```

#### 重启设备

```bash
rkdeveloptool rd
```
	
#### 加载 loader 文件： 

```bash
rkdeveloptool db loaderfile

# output
Downloading bootloader succeeded.
```
其中 `loaderfile` 文件在不同的SoC平台均有所不同，详情请查看[各个平台的 U-Boot image 文件](#spi-镜像及-loader-文件)。 

#### 写入 SPI flash

:::caution
写入 SPI flash 前需要先加载 loader 文件
:::

```bash
sudo  rkdeveloptool wl 0 spiimage

# output
Write LBA from file (100%)
```

其中 `spiimage` 文件每个产品都不同，详情请查看[各个平台的 U-Boot image 文件](#spi-镜像及-loader-文件)。  

#### 烧录主板镜像：  

```bash
rkdeveloptool wl 0 imagefile
```

其中 `imagefile` 为主板所需烧录的 img 镜像，需要在对应产品系列的镜像下载页下载系统镜像后进行解压才能获取。

</TabItem>
</Tabs>

## SPI U-Boot 镜像及 Loader 文件

 - ROCK 3 系列：  
	- [CM3 IO SPI U-Boot 镜像](https://dl.radxa.com/rock3/images/loader/radxa-cm3-io/radxa-cm3-io-idbloader-g8684d740b9f.img)
	- [rk356x_spl_loader_ddr1056_v1.10.111.bin](https://dl.radxa.com/rock3/images/loader/radxa-cm3-io/rk356x_spl_loader_ddr1056_v1.10.111.bin)
	- [rk356x_spl_loader_ddr1056_v1.12.109_no_check_todly.bin](https://dl.radxa.com/rock3/images/loader/rk356x_spl_loader_ddr1056_v1.12.109_no_check_todly.bin)

 - ROCK 4 系列：  
	- [rk3399_loader_v1.27.126.bin](https://dl.radxa.com/rockpi4/images/loader/rk3399_loader_v1.27.126.bin)

 - ROCK 5 系列：  
	- [ROCK 5B SPI U-Boot 镜像](https://dl.radxa.com/rock5/sw/images/loader/rock-5b/release/rock-5b-spi-image-gbf47e81-20230607.img)
	- [ROCK 5A SPI U-Boot 镜像](https://dl.radxa.com/rock5/sw/images/loader/rock-5a/rock-5a-spi-image-g4b32117-20230605.img)
	- [rk3588_spl_loader_v1.08.111.bin](https://dl.radxa.com/rock5/sw/images/loader/rock-5b/rk3588_spl_loader_v1.08.111.bin)




