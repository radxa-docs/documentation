---
sidebar_label: 'Rockchip Dev Tools'
sidebar_position: 2
---

# Introduction

Rockchip provides the [RKDevTool](#RKDevTool) tool as a basic development aid on Windows platforms, 
and the [rkdeveloptool](#rkdeveloptool) for simple development on Linux and Mac platforms.  

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
<TabItem value="RKDevtool" label="Windows: RKDevtool" default>

## RKDevTool

Radxa currently supports the use of the RKDevTool for basic development of devices that have entered Maskrom mode. 
The most common method is to flash the bootloader and to burn the system.

### Windows installation of RKDevTool

The installation on Windows is very simple, just download the development tools and the RK driver and install them directly.  

RK dev tool: [RKDevTool V2.96](https://dl.radxa.com/tools/windows/RKDevTool_Release_v2.96.zip)  
RK official driver: [DriverAssitant V5.0](https://dl.radxa.com/tools/windows/DriverAssitant_v5.0.zip)  

Use the RK Driver Assistant tool to install the driver. 
There is no need to connect your Rockchip device for this process, just download the zip package and extract DriverAssitant V5.0.zip. 
Then double-click DriverInstall.exe to launch the tool and click Install Driver to install the driver. 
If you have already installed the Rockchip USB driver for another Rockchip device, please click on "Uninstall Driver" first.
![RK Driver](/img/configuration/RK-Driver-Assistant-Install-Uninstall.webp)

The following is an example of opening the RKDevTool and connecting into the Maskrom device after the installation is complete:  
![RK Dev interface](/img/rock5a/on-maskrom.webp)

### Burning the system image

1. Open the RKDevTool  
![RKDevTool](/img/configuration/rkdevtool.webp)

2. Connect the device to the host via OTG and boot into Maskrom mode  
:::caution
The way to enter Maskrom mode will vary from board to board, the common method is to remove all storage media, 
connect the OTG cable and power up, enter Maskrom mode and then install the storage media, usually you also need to press the Maskrom button or connect the Maskrom pin.  
:::
![RKDevTool maskrom](/img/configuration/rkdevtool-maskrom.webp)

3. Flash configuration  
:::caution
The image selected here should be in img format, the downloaded image is in zip format by default and needs to be unzipped before flashing.   
:::
In the storage option, select the media you want to install the system on  
![RKDevTool storage](/img/configuration/rkdevtool-storage.webp)  
Select [loader](#spi-u-boot-image-and-loader-file) and the img image file by clicking on the blank cell configuration  
![RKDevTool choose](/img/configuration/rkdevtool-choose.webp) 

4. Check the `Write by Address` box and click `run`  
![RKDevTool flashing](/img/configuration/rkdevtool-flashing.webp)  

5. Wait for the write to complete, then the device will automatically reboot into the system  
![RKDevTool complete](/img/configuration/rkdevtool-complete.webp) 

### Flashing the bootloader to SPI flash

When using an NVME SSD as the system boot disk, you need to flash the bootloader to SPI to boot the system.  

Here are the steps for burning the bootloader using RKDevTool: 

1. Get the [corresponding platform bootloader and U-Boot image](#spi-u-boot-image-and-loader-file). 

2. Enter Maskrom mode on the motherboard and select the corresponding bootloader and SPI image  
![RKDevTool SPINOR](/img/configuration/rkdevtool-spinor.webp) 

3. Click `run` and wait for the write to complete  
![RKDevTool SPI complete](/img/configuration/rkdevtool-spi-complete.webp) 

</TabItem>
<TabItem value="rkdeveloptool" label="Linux/MacOS: rkdeveloptool">

## rkdeveloptool

On Linux/MacOS, there is no dedicated package and we need to compile and build the rkdeveloptool tool from source code. 

### Installing rkdeveloptool on Linux/MacOS

As an example, to build the rkdeveloptool tool on a Debian Linux distribution, follow these instructions:  

1. Install the build dependencies.  

```bash
sudo apt-get install libudev-dev libusb-1.0-0-dev dh-autoreconf pkg-config libusb-1.0 build-essential g++
```

2. Clone the source code and compile it.  

```bash
 git clone https://github.com/rockchip-linux/rkdeveloptool
 cd rkdeveloptool
 autoreconf -i
 . /configure
 make
```

3. Add the rkdeveloptool executable in the current directory to the system tools.

```bash
 sudo cp rkdeveloptool /usr/local/bin/
```

#### Possible problems encountered during installation

Compilation error 1:

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

Then you need to make some changes to the code in `main.cpp`:  

```bash
1490    static inline uint32_t convertChipType(const char* chip) {
1491	    char buffer[5];
1492	    memset(buffer, 0, sizeof(buffer));
1493	    snprintf(buffer, sizeof(buffer), "%s", chip);
1494	    return buffer[0] << 24 | buffer[1] << 16 | buffer[2] << 8 | buffer[3];
1495    }
```

where line 1493 is amended to read:  

```bash
1493        snprintf(buffer, sizeof(buffer), "%.*s", static_cast<int>(sizeof(buffer) - 1), chip);
```

Re-execute the make command again:  

```bash
    make
```

### Use of rkdeveloptool

After typing the `rkdeveloptool` command, you can see the list of supported functions with the following parameters:  

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

The parameters of each function are preceded by a description of its function, and a simple example of its use is given here for commonly used functions:

#### View connected Maskrom devices: 

```bash
rkdeveloptool ld

# output
DevNo=1 Vid=0x2207,Pid=0x350b,LocationID=106 Maskrom
```

#### reboot device

```bash
rkdeveloptool rd
```
	
#### flash loader file: 

```bash
rkdeveloptool db loaderfile

# output
Downloading bootloader succeeded.
```
The ``loaderfile`` file varies between SoC platforms, see [U-Boot image file for each platform](#spi-u-boot-image-and-loader-file) for details.  

#### Writing to SPI flash

:::caution
You should flash loader file first before writing to SPI flash.
:::

```bash
sudo rkdeveloptool wl 0 spiimage

# output
Write LBA from file (100%)
```

where the `spiimage` file is different for each product, see [U-Boot image files for each platform](#spi-u-boot-image-and-loader-file) for details.  

#### To flash the system OS image:  

```bash
rkdeveloptool wl 0 imagefile
```

:::caution
where `imagefile` is the img image to be burned for the motherboard, which needs to be unpacked after downloading the system image from the image download page of the corresponding product family.
:::

</TabItem>
</Tabs>

## SPI U-Boot image and Loader file

 - ROCK 3 Series:  
	- [CM3 IO SPI U-Boot image](https://dl.radxa.com/rock3/images/loader/radxa-cm3-io/radxa-cm3-io-idbloader-g8684d740b9f.img)
	- [rk356x_spl_loader_ddr1056_v1.10.111.bin](https://dl.radxa.com/rock3/images/loader/radxa-cm3-io/rk356x_spl_loader_ddr1056_v1.10.111.bin)
	- [rk356x_spl_loader_ddr1056_v1.12.109_no_check_todly.bin](https://dl.radxa.com/rock3/images/loader/rk356x_spl_loader_ddr1056_v1.12.109_no_check_todly.bin)

 - ROCK 4 Series:  
	- [rk3399_loader_v1.27.126.bin](https://dl.radxa.com/rockpi4/images/loader/rk3399_loader_v1.27.126.bin)

 - ROCK 5 Series:  
	- [ROCK 5B SPI U-Boot image](https://dl.radxa.com/rock5/sw/images/loader/rock-5b/release/rock-5b-spi-image-gbf47e81-20230607.img)
	- [ROCK 5A SPI U-Boot image](https://dl.radxa.com/rock5/sw/images/loader/rock-5a/rock-5a-spi-image-g4b32117-20230605.img)
	- [rk3588_spl_loader_v1.08.111.bin](https://dl.radxa.com/rock5/sw/images/loader/rock-5b/rk3588_spl_loader_v1.08.111.bin)



