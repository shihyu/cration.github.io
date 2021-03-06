---
layout: post
title: "用PWM实现音频播放"
description: "　　实现了一个用PWM来播放音频的方案。"
category: "electronic"
tags: [电路]
---
{% include JB/setup %}

　　用单片机音频播放的一个常用方法是用DAC输出音频数据，经过音频放大器后驱动喇叭来播放声音。如果所用的单片机没有DAC外设的话，我们也可以使用PWM口来模拟ADC的输出。

　　基本思路：用PWM经过低通滤波器来输出模拟电压，从而实现ADC的功能。

![原理框架]({{site.img_path}}/PWM_as_DAC_1.png)

　　对于频率计算，有以下几个要点。

　　①截止频率要远低于PWM频率，否则PWM带来的噪声将非常严重。一般认为PWM频率是RC低通滤波器截止频率的十倍以上是比较合适的。当然，截止频率不能低于输出音频的频率，否则将丢失信息。

　　②根据奈奎斯特定理，一般建议低通滤波器的截止频率设置为输出音频采样率的一半。常用音频的采样率有：8000、11025、22050、44100Hz。

　　③在不影响分辨率的情况下，PWM频率越高越好。

　　RC低通滤波器的截止频率计算方法：

$$f=\frac{1}{2\pi RC}$$

　　对于主频较低的单片机来说，兼顾PWM频率和分辨率是很难的，因为时钟频率有限。因此，可以采用如下方式，用并行输出两路数据合成DAC的方式来解决这个问题。

![双路PWM]({{site.img_path}}/PWM_as_DAC_2.png)

　　或者，更为丧心病狂的三路PWM合成。

![三路PWM]({{site.img_path}}/PWM_as_DAC_3.png)

　　对于双路PWM合成，有一个高精度同样也更加复杂的方案：

![高精度双路PWM]({{site.img_path}}/PWM_as_DAC_4.png)

　　对上面这个电路的具体分析请参考页面底端的参考链接，这里就不再赘述了。

　　足够高的PWM频率可以保证较低的噪声和失真，但在PWM频率较低时，必须考虑信号失真的问题。下面给出一个方案，用积分电路和两条控制线来解决这个问题。

![积分采样电路]({{site.img_path}}/PWM_as_DAC_5.png)

　　以上电路的工作流程：

* 准备开始积分，闭合左侧开关，清空积分电容。
* 开始积分，断开左侧开关，积分电路工作。
* 积分结束，闭合右侧开关，左侧运放给采样电容充电。
* 断开右侧开关，采样电容保持一段时间电压。此时可以闭合左侧开关清空积分电容。
* 重复以上步骤。

-------------------------------------------

###References

[STR7/STR9 audio generation with PWM](http://www.st.com/st-web-ui/static/active/cn/resource/technical/document/application_note/CD00119860.pdf)  
[Arduino (ATmega) PWM audio DAC](http://wiki.openmusiclabs.com/wiki/PWMDAC)  
[PWM Distortion](http://www.openmusiclabs.com/learning/digital/pwm-dac/pwm-distortion-analysis/)  
[PWM as DAC](http://www.openmusiclabs.com/learning/digital/pwm-dac/)  
[Dual PWM Circuits](http://www.openmusiclabs.com/learning/digital/pwm-dac/dual-pwm-circuits/)  
[Combine two 8-bit outputs to make one 16-bit DAC](http://m.eet.com/media/1134628/15421-93004di.pdf)  
