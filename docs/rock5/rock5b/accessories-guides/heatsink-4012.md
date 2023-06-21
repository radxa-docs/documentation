---
sidebar_label: 'Radxa Heatsink 4012 使用教程'
sidebar_position: 40
---

# Heatsink 4012 使用教程

## 安装

Heatsink 4012是专门为ROCK 5B量身定制的散热风扇，安装教程如图所示：

- 将散热硅胶贴在散热器上；  

![Heatsink 4012](/img/rock5b/heatsink4012-use-1.webp)  

- 把散热硅胶涂在芯片上；
   
![Heatsink 4012](/img/rock5b/heatsink4012-use-2.webp) 

- 将卡扣对准ROCK 5B的露铜区按压下去；  
- 连接电源线就完成了。  

![Heatsink 4012](/img/rock5b/heatsink4012-use-3.webp) 



## 配置

系统默认是有三种模式    
- **power_allocator**：系统默认是无风扇模式或DC风扇模式。确保机器在没有散热风扇的前提下依旧能稳定工作；  
- **user_space**：手动控制散热风扇模式。用户可以根据自己的需要，通过命令终端控制散热风扇的转速；  
- **step_wise**：自动温度调节模式。CPU在60℃以下散热风扇处于休眠状态，当CPU达到60℃以上散热风扇开始工作。  
**注意：当ROCK 5B处于关机和睡眠状态的时候，散热风扇将不工作。**  

你可以通过命令终端"`retsup`->`Hardware`->`Thermal governor`"，用`空格键`选用模式，具体操作如下：

同时按“Ctrl + Alt + T”打开终端，运行“rsetup”命令如下：

```bash
radxa@rock-5b:~$ rsetup
```

输入密码并选择`Hardware` 进入硬件控制端界面:  

```bash
Please select an option below:
        System Maintaince
        Hardware 
        Overlays
        Connectivity
        User Settings
        Localization
        About
        <Ok>            <Cancel>  
```

按然后进入 `Thermal governor`
```bash
Manage on-board hardware: 
        Video capture devices
        GPIO LEDs       
        Thermal governor
        Configure DSI display mirroring
        <Ok>            <Cancel>       
```
用`空格键`选用模式
  
```  bash
┌─────────────────────────────────────────┤ RSETUP ├───────────────────────────────────────────────┐
│ Please select the thermal governor.                                                              │
│ Recommendation: fanless or DC fan => power_allocator | PWM fan => step_wise                      │
│                                                                                                  │
│    (*) power_allocator                                                                           │
│    ( ) user_space                                                                                │
│    ( ) step_wise                                                                                 │
│    ( ) fair_share                                                                                │
│                                                                                                  │
│                                                                                                  │
│                                                                                                  │
│                                                                                                  │
│                                                                                                  │
│                                                                                                  │
│                                                                                                  │
│                                                                                                  │
│                                                                                                  │
│                                                                                                  │
│                                 <ok>                        <Cancel>                             │           
└──────────────────────────────────────────────────────────────────────────────────────────────────│   
```

如果你选用的是`user_space`模式，你需要手动去控制散热风扇。

首先，你要找到风扇设备节点 `pwm-fan`:

```bash
cat /sys/class/thermal/cooling_device*/type
```

举个例子，该散热风扇的挂载在 `cooling_device1`，你将会通过`cat /sys/class/thermal/cooling_device1/type`查看到`pwm-fan`: 
```bash
radxa@rock-5b: cat /sys/class/thermal/cooling_device1/type
pwm-fan
```

**注意：以下操作以`cooling_device1`为例**

直接打开散热风扇最高速度:
```
radxa@rock-5b:~$ sudo cp /sys/class/thermal/cooling_device1/max_state /sys/class/thermal/cooling_device1/cur_state
```
您可以通过以下指令查看散热风扇支持多少转速：
```
radxa@rock-5b:~$ cat /sys/class/thermal/cooling_device1/max_state
4
```

如果要选择第三档速度，请按照以下指令进行操作：
```
echo 3 | sudo tee /sys/class/thermal/cooling_device1/cur_state
```

如果您想使用其他速度档位，只需更改数字即可。 如果要关闭散热风扇，速度档选0即可。
```
echo 0 | sudo tee /sys/class/thermal/cooling_device1/cur_state
```