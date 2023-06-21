---
sidebar_label: '概览'
sidebar_position: 10
---

# Overview

Radxa ROCK 3 计算模块 (CM3) 是基于 Rockchip RK3566 片上系统 (SoC) 的系统模块 (SoM)。 CM3 集成了中央处理器 (CPU)、电源管理单元 (PMU)、DRAM 内存、闪存和无线连接（WiFi 5 和 BT 5.0），外形小巧，仅为 55 毫米 x 40 毫米。 
ROCK CM3核心板使用3x 100P 0.4mm 间距 B2B 连接器，提供强大的功能支持，可与客户的底板结合，构建完整的产品功能，从而加快研发进程。

<div class='rock_cm3_img'>

![ROCK CM3](/img/cm3/radxa_cm3_top_bottom.webp)

</div>

ROCK CM3 提供各种 LPDDR4 RAM 和 eMMC 尺寸配置：  
- RAM: 1 / 2 / 4 / 8 GB  
- eMMC: NA / 8 / 16 / 32 GB  

同时，我们提供[瑞莎 CM3 接口板](/accessories/cm3-io-board)与ROCK CM3相兼容，你可以使用它开发出你想要的产品。例如，HMI，机器人，可视对讲机，智能家居等。

<div class='rock_cm3_install_io_board'>

![ROCK CM3](/img/cm3/cm3-install-io-board.webp)

</div>

## 特点

### 硬件

- CPU：四核 Arm® Cortex®‑A55 (ARMv8) 64 位 @ 2.0GHz
- GPU：Arm Mali™‑G52‑2EE、OpenGL® ES1.1/2.0/3.0/3.1/3.2、Vulkan® 1.1、OpenCL™ 2.1
- NPU：1个TOPs@INT8，支持INT8、INT16、FP16、BFP16，支持TensorFlow、Caffe、Tflite、Pytorch、Onnx、Android™ NN等深度学习框架
- 内存：32 位 LPDDR4X 高达 8GB
- 存储：eMMC 5.1 高达 512GB
- 显示：单显示引擎、HDMI2.0、eDP 1.3、双 MIPI‑DSI、单 LVDS 组合、24 位 RGB/BT1120 和 EBC 接口
- 多媒体：4K H.265/H.264/VP9 视频解码器和 1080p@60fps H.264/H.265 视频编码器
- 视频输入：800 万像素 ISP 和 1 x 4 通道或 2 x 2 通道 MIPI CSI‑2 和 DVP 接口
- 音频接口：8 通道 I2S0/I2S1、2 通道 IS2/I2S3、SPDIF0、8 通道 PDM0、8 通道 TDM 和语音活动检测 (VAD)
- 高速接口：一个 USB 3.0 主机、两个 SATA 3.0、一个 PCIe 2.1、两个 SerDes（串行
模拟器/解串器）通道、双 USB 2.0 主机和一个 USB 2.0 OTG
- 以太网：1000M 以太网物理层 (RTL8211F)
- 安全性：Arm TrustZone® 安全扩展、安全视频路径、安全 JTAG 调试、安全启动、OTP 和加密 (AES/TDES/SM4/SM3/SHA256/SHA512/RSA)

### 接口介绍

- 802.11 b/g/n/ac 无线局域网 (Wi‑Fi 5)
- 蓝牙 5.0 
- 8 个 I2C
- 4 个 SPI
- 8 个串口
- 9 个脉宽调制
- 50 个通用输入输出
- 2 个模数转换器
- 1 个千兆以太网 PHY
- 1 x PDM 支持麦克风阵列
- 1 个 I2S
- 2 个 SATA
- 1 x PCIe 2.0、1 通道主机 (5Gbps)
- 1 个 USB 2.0
- 1 个 USB 2.0 OTG
- 1 个 USB 3.0 (5Gbps)
- 1 个标清输入输出 3.0
- 1 x HDMI 高达 4K x 2k@60Hz
- 1 x eDP 四通道（每通道 2.7 Gbps）
- 2 x MIPI DSI @ 1.6Gbps 每通道
- 2 x 2 通道或 1 x 4 通道 MIPI CSI 相机端口
- 1 x LVDS 组合四通道，带 MIPI DSIO 的多路复用器
- 3 x 100 针 0.4 毫米间距 B2B 连接器

### 软件
- Debian/Ubuntu Linux support
- Android 11/12 support






