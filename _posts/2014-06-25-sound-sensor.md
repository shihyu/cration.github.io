---
layout: post
title: "声音强度检测电路"
description: "　　用一个简单的电路实现了对声音强度的检测。"
category: "electronic"
tags: [运放]
---
{% include JB/setup %}

　　今日的工作中，参考了一些资料，设计了如下电路，实现了对声音强度的检测。

![原理图]({{site.img_path}}/sound_sensor_schematic.png)

### 工作原理

　　麦克风将声音信号转换为电信号后，经过电容耦合，输入放大器。运放的正输入端偏置为1.2V，反相放大100倍。超出运放工作范围的信号将失真，但这不影响我们的使用。

　　放大后的信号进过一个二极管检波电路，将调幅包络检出，然后经过一个电压跟随器输出。

　　注意到反相放大器和检波电路之间有一个51Ω的电阻。若是将电阻改为导线连接，则输出信号将会有一个尖峰噪声，产生原因未知，猜测可能与“闪变噪声”有关。

-------------------------------------------

###References

[声音检测电路图](http://www.elecfans.com/dianzichangshi/2009032938890.html)  


