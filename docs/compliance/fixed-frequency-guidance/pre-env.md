---
sidebar_label: 'Fixed-frequency Guidance'
sidebar_position: 13
---

When we do certification, if the product has an on-board wifibt module, we need to prepare the board with fixed frequency.

We have done some module certifications, such as AP6212 AP6256 CM256 ...

The following is an example of how to prepare a fixed frequency environment with ap6256.

# 1.Tools
The [wl](https://dl.radxa.com/fix_freq_docs/wl) tool for wifi, the hcitool for bt.

Prepare the wl tool, push it to the system by adb or scp, grant executable privileges, and put it under /usr/local/sbin.

```
$ adb push wl /home/
or
$ scp wl radxa@ip:/home/
$ chmod +x wl
$ sudo cp /home/wl /usr/local/sbin
```

# 2.Specific firmware 
Download this firmware: [fw_bcm43456c5_ag_mfg.bin](https://dl.radxa.com/fix_freq_docs/ap6256/fw_bcm43456c5_ag_mfg.bin)
```
# Disable the brcmfmac driver by default
echo "blacklist brcmfmac" >> /etc/modprobe.d/blacklist.conf

# Replace firmware
$ adb push fw_bcm43456c5_ag_mfg.bin /home
$ cd /lib/firmware/brcm
$ cp fw_bcm43456c5_ag.bin fw_bcm43456c5_ag.bin.bkp
$ cp fw_bcm43456c5_ag_mfg.bin fw_bcm43456c5_ag.bin
$ reboot
$ sudo wl ver                       #The word WLTEST appears in the version information, which means the wifi firmware is successfully replaced.
$ sudo hcitool cmd 0x03 0x003       # No errors are reported, which means that bt is also OK.
```

# 3.Fixed frequency command
Refer to the following documentation for testing.

[Wi-Fi RF Test Commands for Linux-v05.pdf](https://dl.radxa.com/fix_freq_docs/Wi-Fi+RF+Test+Commands+for+Linux_BCM4339-v05.pdf)

[BT+RF+Test+Commands+for+Linux-v07.pdf](https://dl.radxa.com/fix_freq_docs/BT+RF+Test+Commands+for+Linux-v07.pdf)

# 4.Others
Other modules are similar, just replace specific firmware.

AP6212: cp [fw_bcm43438a1_mfg.bin](https://dl.radxa.com/fix_freq_docs/ap6212/fw_bcm43438a1_mfg.bin) fw_bcm43438a1.bin

CM256: cp [cyw43455-mfgtest-7.45.100.18.bin](https://dl.radxa.com/fix_freq_docs/cm256/cyw43455-mfgtest-7.45.100.18.bin) fw_cyw43455.bin

# 5.Note
When you test wifi, you need to turn off bt:
```
ifconfig wlan0 up
hciconfig hci0 down
```
When you test bt, you need to turn off wifi:
```
hciconfig hci0 up
ifconfig wlan0 down
```