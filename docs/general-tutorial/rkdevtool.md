---
sidebar_label: 'RK 开发工具的使用'
sidebar_position: 2
---

# 简介

Rockchip 在 Windows 平台上提供了 [RKDevTool](rk-dev-tool#windows) 工具作为基础开发辅助工具， 而在 Linux 和 Mac 平台下，则需要使用 [rkdeveloptool](rk-dev-tool#linuxmacos) 来进行简单开发。  

## RKDevTool

目前瑞莎支持使用 RKDevTool 来对瑞莎进入 Maskrom 的设备进行基础开发操作， 
其中最常用的便是烧写 BootLoader 以及烧写系统(该方式常被称为：线刷)。

### 烧写系统

1. 打开RKDevTool  
![RKDevTool zh](/img/configuration/rkdevtool-zh.webp)

2. 将设备启动进入 Maskrom 模式  
**注意：每个主板进入 Maskrom 模式的方式会有所区别，通用的方法为：移除所有存储介质，连接OTG线并上电，进入 Maskrom 模式后再安装存储介质。**  
![RKDevTool zh maskrom](/img/configuration/rkdevtool-zh-maskrom.webp)

3. 烧录配置  
**注意：此处选择的镜像应该为 img 格式，下载默认为压缩包，需要解压缩后再进行烧录。**  
在 storage 选项中选择需要安装系统的介质  
![RKDevTool zh storage](/img/configuration/rkdevtool-zh-storage.webp)  
点击空白单元格配置选择 [loader](#spi-镜像及-loader-文件) 和 img 镜像文件 
![RKDevTool zh choose](/img/configuration/rkdevtool-zh-choose.webp) 

4. 勾选强制按地址写后点击执行
![RKDevTool zh flashing](/img/configuration/rkdevtool-zh-flashing.webp) 

5. 等待烧写完成，随后设备自动重启进入系统  
![RKDevTool zh complete](/img/configuration/rkdevtool-zh-complete.webp) 

### 烧写 Bootloader 到 SPI flash

在使用 NVME SSD 作为系统启动磁盘的时候，需要在 SPI 中烧录 Bootloader 来引导系统。  

以下是使用 RKDevTool 烧录 Bootloader 的步骤：  

1. 获取[对应平台 bootloader 和 u-boot image](#spi-镜像及-loader-文件)  

2. 主板进入 Maskrom 模式， 选择对应的 bootloader 和 SPI image  
![RKDevTool zh SPINOR](/img/configuration/rkdevtool-zh-spinor.webp) 

3. 点击执行，等待烧写完成  
![RKDevTool zh SPI complete](/img/configuration/rkdevtool-zh-spi-complete.webp) 



## rkdeveloptool

可根据教程安装 [rkdeveloptool](rk-dev-tool#linuxmacos)，在键入`rkdeveloptool`命令后，可以看到其支持的功能列表参数：

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

### 查看已连接的 Maskrom 设备： 

```bash
rkdeveloptool ld

DevNo=1 Vid=0x2207,Pid=0x350b,LocationID=106 Maskrom
```

### 重启设备

```bash
rkdeveloptool rd
```
	
### 加载 loader 文件： 

```bash
rkdeveloptool db loaderfile

Downloading bootloader succeeded.
```
其中 `loaderfile` 文件在不同的SoC平台均有所不同，详情请查看[各个平台的 u-boot image 文件](#spi-镜像及-loader-文件)。 

### 写入 SPI flash

```bash
sudo  rkdeveloptool wl 0 spiimage

Write LBA from file (100%)
```

其中 `spiimage` 文件每个板子都不同，详情请查看[各个平台的 u-boot image 文件](#spi-镜像及-loader-文件)。  

### 将镜像烧录进主板：  

```bash
rkdeveloptool wl 0 imagefile
```

其中 `imagefile` 为主板所需烧录的镜像，可在对应产品系列的镜像下载页下载系统镜像。

## SPI 镜像及 Loader 文件

 - ROCK 3 系列：  
	- [CM3 IO SPI u-boot 镜像](https://dl.radxa.com/rock3/images/loader/radxa-cm3-io/radxa-cm3-io-idbloader-g8684d740b9f.img)
	- [rk356x_spl_loader_ddr1056_v1.10.111.bin](https://dl.radxa.com/rock3/images/loader/radxa-cm3-io/rk356x_spl_loader_ddr1056_v1.10.111.bin)
	- [rk356x_spl_loader_ddr1056_v1.12.109_no_check_todly.bin](https://dl.radxa.com/rock3/images/loader/rk356x_spl_loader_ddr1056_v1.12.109_no_check_todly.bin)

 - ROCK 4 系列：  
	- [rk3399_loader_v1.27.126.bin](https://dl.radxa.com/rockpi4/images/loader/rk3399_loader_v1.27.126.bin)

 - ROCK 5 系列：  
	- [ROCK 5B SPI u-boot 镜像](https://dl.radxa.com/rock5/sw/images/loader/rock-5b/release/rock-5b-spi-image-gbf47e81-20230607.img)
	- [ROCK 5A SPI u-boot 镜像](https://dl.radxa.com/rock5/sw/images/loader/rock-5a/rock-5a-spi-image-g4b32117-20230605.img)
	- [rk3588_spl_loader_v1.08.111.bin](https://dl.radxa.com/rock5/sw/images/loader/rock-5b/rk3588_spl_loader_v1.08.111.bin)




