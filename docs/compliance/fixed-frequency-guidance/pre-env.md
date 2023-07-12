---
sidebar_label: '定频指导'
sidebar_position: 13
---

# 简介  

当我们在做CE或者FCC认证的时候，如果产品有板载 WiFi/BT 模块，我们就需要固定频率。

我们已经做过了一些模块的认证，如 AP6212 AP6256 CM256 等。

下面以 AP6256 为例说明如何准备固定频率环境。

## 1.工具

[ wl 工具](https://dl.radxa.com/fix_freq_docs/wl)是用于 WiFi 测试使用，需要下载并复制到系统里，使用系统自带的 hcitool 工具来测试 bt。

准备好 wl 工具，通过 adb push 或 scp 将其推送到系统的 `/usr/local/sbin` 路径下并授予可执行权限。

```
$ adb push wl /home/
or
$ scp wl radxa@ip:/home/
$ chmod +x wl
$ sudo cp /home/wl /usr/local/sbin
```

## 2.特定固件 

下载该固件：[fw_bcm43456c5_ag_mfg.bin](https://dl.radxa.com/fix_freq_docs/ap6256/fw_bcm43456c5_ag_mfg.bin)

```
# Disable the brcmfmac driver by default
echo "blacklist brcmfmac" >> /etc/modprobe.d/blacklist.conf

# Replace firmware
$ adb push fw_bcm43456c5_ag_mfg.bin /home
$ cd /lib/firmware/brcm
$ cp fw_bcm43456c5_ag.bin fw_bcm43456c5_ag.bin.bkp
$ cp fw_bcm43456c5_ag_mfg.bin fw_bcm43456c5_ag.bin
$ reboot
$ sudo wl ver                       # 出现 WLTEST 字样，说明 firmware 替换成功。
$ sudo hcitool cmd 0x03 0x003       # 没有报错，说明 bt 环境正常
```

## 3.定频指令

请参考以下文档进行测试。

[Wi-Fi RF Test Commands for Linux-v05.pdf](https://dl.radxa.com/fix_freq_docs/Wi-Fi+RF+Test+Commands+for+Linux_BCM4339-v05.pdf)

[BT+RF+Test+Commands+for+Linux-v07.pdf](https://dl.radxa.com/fix_freq_docs/BT+RF+Test+Commands+for+Linux-v07.pdf)

## 4.其他

其他型号的模组准备过程和上述类似，需要替换成相对应的 firmware。

AP6212: cp [fw_bcm43438a1_mfg.bin](https://dl.radxa.com/fix_freq_docs/ap6212/fw_bcm43438a1_mfg.bin) fw_bcm43438a1.bin

CM256: cp [cyw43455-mfgtest-7.45.100.18.bin](https://dl.radxa.com/fix_freq_docs/cm256/cyw43455-mfgtest-7.45.100.18.bin) fw_cyw43455.bin

## 5.注意事项

测试WIFI时，需要关闭BT：

```
ifconfig wlan0 up
hciconfig hci0 down
```

测试BT时，需要关闭WIFI：

```
hciconfig hci0 up
ifconfig wlan0 down
```