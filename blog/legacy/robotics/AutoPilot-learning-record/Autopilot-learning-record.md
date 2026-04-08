---
title: 自动驾驶相关组件的学习记录
date: 2025-04-17
description: 更新 ing | 更新完 PID 部分
tags: 
  - 机器人
---
# 自动驾驶相关的组件学习记录

## 参考链接

PID 视频教程
- b 站搜索 PID 关键词有很多 

Arduino PID 库作者的博客，源码仓库
-  [br3ttb/Arduino-PID-Library (github.com)](https://github.com/br3ttb/Arduino-PID-Library)
-  [Introducing Proportional On Measurement | Project Blog (brettbeauregard.com)](http://brettbeauregard.com/blog/2017/06/introducing-proportional-on-measurement/)
-  [pid | Search Results | Project Blog (brettbeauregard.com)](http://brettbeauregard.com/blog/?s=pid)
- [中文翻译](https://www.cnblogs.com/FZLGYZ/p/11696252.html)

调参助手
- 安利 VOFA+
## 闭环控制和控制量

首先需要了解一些闭环控制、开环控制的概念，以及对应的使用场景和作用

然后得要明白在一个系统中要控制的对象是什么属于量，要明白这个量的类型是什么（自衡对象还是积分对象）
```

```
## PID 闭环控制相关的概念

$\displaystyle PID$ 算法的数学表达形式具体有几种类型，这个知道了解一下就行，**在这里建立 $\displaystyle PID$ 算法的感性认识就行了**
> 值得一提的是一般的 $MCU$ 是离散系统，离散系统时间横轴最小变化量为 1，除以 1 等于没除


**$\displaystyle P$ 算法就像挂在预期值上的一根弹簧**，因为 $\displaystyle K_{p}e_{k}$ 有点像弹簧的胡克定律 $\displaystyle F=-kx$ 嘛。它总有把实际值往预期值位置拉的能力。这里的 $\displaystyle e_{k}$ 就是预期值和实际值之差的绝对值，表示的是实际值和预期值之间的误差。**由 $\displaystyle K_{p}e_{k}$ 可以看出来误差如果越大，那么 $\displaystyle P$ 算法的输出就越大。进而用增大的 $\displaystyle P$ 算法输出去减小实际值和预期值之间的误差，让实际值值不断接近预期值**
> $\displaystyle P$ 参数 $\displaystyle K_{p}$ 越大，弹簧的恢复能力就越大。但是 $\displaystyle P$ 参数不是越大越好，如果 $\displaystyle P$ 参数越大，那很容易就恢复过头，**实际值就像弹簧一样在预期值附近振荡，不能很好的快速稳定**，这种实际值在预期值附近振荡的现象就是**弹簧过冲现象**


$\displaystyle D$ 算法就像阻尼，所谓的阻尼指的就是阻碍系统的变化的东西，就跟楞次定律一样，和误差变化趋势相反。因为 $\displaystyle K_{d}(e_{k}-e_{k-1})=K_{d} \frac{e_{k}-e_{k-1}}{1}$，$\displaystyle (e_{k}-e_{k-1})$ 是两次误差之差，和系统响应 (也就是误差调整的速度) 有关，所以 $\displaystyle D$ 算法和水的阻力 $\displaystyle F=kv$ 很像，它就好像放在水里的弹簧，弹簧的速度越快，受到水的阻力越大，越难继续恢复。因此可以把 $\displaystyle D$ 算法理解为阻尼，抑制过冲现象

**如果误差值 $\displaystyle e_{k}$ 很大或者 $\displaystyle P$ 参数很大，那么 $\displaystyle P$ 算法的输出就会很大，进而导致系统剧烈响应，也就是两次误差之差 $\displaystyle (e_{k}-e_{k-1})$ 很大，进而用输出变大的 $\displaystyle D$ 算法来阻碍系统的变化，使得输出停在预期值而不过冲**。由于 $\displaystyle (e_{k}-e_{k-1})$ 有正有负，所以要灵活调节 $\displaystyle K_{d}$ 使得 $\displaystyle P$ 算法输出和 $\displaystyle D$ 算法输出的效果反相
> 实际情况中，如果 $\displaystyle D$ 参数过大，会产生一种高频率小幅度的震荡，就像在抽搐一样。这是因为如果 $\displaystyle D$ 参数太大，那么系统的任何轻微变化都会引起 $\displaystyle D$ 算法的强烈响应

$\displaystyle I$ 算法是误差的累加。由 $\displaystyle K_{i} \sum \limits_{j=1}^{k}e_{j}$ 可以看出，只要存在误差 (这种误差叫稳态误差)，不论误差有多小，$\displaystyle I$ 的输出也会像滚雪球样越滚越大。它输出是作用是就是消除稳态误差。因为当系统误差已经接近 0 时，$\displaystyle P$ 的输出会很小，起不到继续减小误差的作用，导致误差始终没办法减小到 0。这个时候就需要用到 $\displaystyle I$ 算法让误差值不断累加并将累加后的值输出，进而消除稳态误差。这就是更精确的控制

但是有时候，加入 $I$ 算法的系统，会不太稳定。因为误差是不断累加的，加上 $\displaystyle I$ 加的次数太多而没有给 $\displaystyle I$ 的输出限幅的话，系统容易崩。一般实际使用的时候稳态误差还是处在可以接受的范围，所以没有特别需要的话，$PD$ 控制就可以了。实在要使用 $\displaystyle I$ 算法的话，要限制误差累加的幅度，限制最大值和最小值
> 如果使用超调过冲的参数整定方式，一般先调 $\displaystyle P$ 然后是 $\displaystyle D$，几乎不会动 $\displaystyle I$
> 但是如果使用更流程化的 `Lambda` 参数整定方法，直接可以得出 $P$ 和 $\displaystyle I$ 了，不需要用到 $\displaystyle D$ 即可有不错的效果


总而言之，$\displaystyle P$ 是根据现在的误差位置控制，$\displaystyle D$ 又是根据未来控制（微分表示误差变化趋势 ）$\displaystyle I$ 是根据过去的控制（积分误差过去的累积）

## $\displaystyle PID$ 控制器和系统的概念梳理

有了上面对 $\displaystyle PID$ 控制算法的感性认知，接下来就要确定一下实际系统中各个物理量的名称和作用

实际上控制器输出不一定是直接作用到系统上面的，要经过限制幅度（设置量程范围）以及进行一些转化（把正数和负数进行处理，或者加上一些系统特性相关的偏移量）
> 常见的系统特性偏移量有漏电流和电机驱动的死区 PWM 值

![](./Autopilot-learning-record/image-3.png)

![](./Autopilot-learning-record/image-4.png)

## $\displaystyle PID$ 控制器输出的量程问题

一般要对 $\displaystyle PID$ 控制器进行积分限幅和输出限幅

除此之外还要考虑位置式 $\displaystyle PID$ 输出计算出来是负数的情况
> 对于电机可以考虑正数正转，负数反转
> 对于 $\displaystyle DAC$ 输出可以考虑负电压（当然没有负电压源，直接输出 0 也行，就是反向超调的作用不如正向超调，调控能力被削弱了）

## 给 $\displaystyle MCU$ 添加控制器时进入控制器的频率问题

$\displaystyle PID$ 的算法的**计算**要和传感器的数据**读取**紧密放在一起，**读取和计算不要分开**
> $\displaystyle PID$ 处理传感器数据的时间间隔一定要很短，大概 $\displaystyle 5ms$ 到 $\displaystyle 10ms$ 其次每一次 $\displaystyle PID$ 处理传感器数据的时间间隔要一样，否则 $\displaystyle D$ 算法和 $\displaystyle I$ 算法会出问题


下面有几种给 $\displaystyle MCU$ 添加 $\displaystyle PID$ 控制器的方案
- 如果实现的需求和任务种类不多，**采用裸机的话**，一般是在传感器的硬件中断
	- 如果传感器有自带中断的话就可以放在对应的那里，这样最好了
	- 否则就单开一个间隔很短的定时器中断
- 最好是使用 $\displaystyle RTOS$ 实时操作系统，这样可以创建一个任务，实现固定频率进入 $\displaystyle PID$ 控制器进行计算，也可也优雅地管理多任务以及任务之间的数据通信


对于电机而言，最常用的就是磁编码器或者光电编码器这种正交编码器

## 增量式 $\displaystyle PID$ 解决位置式 $\displaystyle PID$ 输出限幅后震荡问题

普通的 $\displaystyle PID$ 是位置式的，就是 $\displaystyle PID$ 控制器输出值是**直接进行限幅和转化**来去作用到系统上的
有时候会因为各种原因，导致控制器直接计算出来的值总是超越限幅幅度，进而导致输出震荡

常见的原因主要是系统的变化和响应太快了，传感器采样和输出作用到系统的时间跟不上
- **系统变化太快**，控制器输出量 $\displaystyle OP$ 刺激被控的过程变量 $\displaystyle PV$ 的阶跃响应太快，纯滞后时间太短
- **传感器采样速率不够快**，跟不上系统变化的速率，导致采集得到的数据突变很大，造成 $\displaystyle PID$ 控制器计算的输出量不匹配，甚至
- **输出量 $\displaystyle OP$ 作用到系统这一过程消耗时间太大了**，导致 $\displaystyle PID$ 计算出来的输出量数值没有及时地作用到系统上面，造成很大的稳态余差或者极大的震荡

面对变化和响应极快的系统，如硬件电路系统，一种解决思路是提高 $\displaystyle PID$ 控制器的运作频率来改善（**提高传感器采样速率 + 减小输出作用时间消耗**）

如果实在没办法，其实是大概率没办法，就可以使用减缓系统变化太快的思路来处理这个震荡问题

一种思路是直接使用增量式 $\displaystyle PID$，另外一种思路是使用低通滤波过滤采样数据
> 增量式 $\displaystyle PID$ 就是 $\displaystyle PID$ 控制器输出值**加上**上一次作用到系统的值去进行限幅和转化才作用到系统

![](./Autopilot-learning-record/image-2.png)

## $\displaystyle PID$ 参数的 Lambda 整定

所谓的 Lambda 整定法，是在开环状态下把作用到系统的输出量 $\displaystyle OP$ 从最小值阶跃到最大值（横跨整个量程时），采样记录要控制的过程变量 $\displaystyle PV$ 被触发的对应阶跃响应曲线

根据过程变量 $\displaystyle PV$ 不同的特性可以把它分为自衡对象和积分对象

注意，开环阶跃响应测数据的采样频率，要和实际 $\displaystyle PID$ 控制器运作时的频率大致相当
> 如果系统变化的太快，使用普通的采样时间得不到阶跃曲线，计算不出时间常数，就无法使用 Lambda 整定
> 
> 如果强行压缩到极短的采样时间，正常运行也要用一样的极快采样率，否则会出现问题

Lambda 整定的 $\displaystyle PID$ 参数计算见：[PID-Lambda-param-tuning](https://github.com/skyswordx/PID-Lambda-param-tuning)
> 我自己写了 lambda 整定的辅助脚本，只需要用 VOFA 串口获取数据导出 excel 即可


## $\displaystyle MATLAB-simulink$ 仿真和辅助整定 $\displaystyle PID$

Matlab 仿真和自动调参-哔哩哔哩 https://b23.tv/eJlG5b6
- 开源了控制的完整程序： ①常规 PID ②改进式 PID ③MATLAB 系统辨识传递函数 ④Simulink 仿真&PID Tuner 
- 自动调参文章地址: https://ittuann.github.io/2021/08/30/Car.html 
- 程序开源: https://github.com/ittuann/Enterprise_E

华南虎也有一个视频

使用 simulink 时，如果遇到使用某个 app 出问题，看看是不是模型文件的名称不对劲，编译一下试试，或者直接改一个名字
有时候是因为名称被占用了

## 对电机三环的 $\displaystyle PID$ 整定

位置环的对象是积分对象，在目标值设定不大的时候，用积分对象自带的积分作用就可以有不错的效果。但是一旦目标值大的时候，用积分对象自带的就不够用了，会有积分作用弱的表现（有稳态余差）

![](./Autopilot-learning-record/image-0.png)

> 顺带一题，稳态鱼叉有可能是因为设定值是本来设定的就有问题，比如电机最大转速参数虚标，或者顺时针转逆时针转的效果不一样

![](./Autopilot-learning-record/image-1.png)

速度环和位置环（内核是普通并联式 $\displaystyle PID$）就已经可以最快达到想要的控制了 (因为一上来 $PWM$ 都是拉满)
单纯的控制性能角度上讲，单独的环已经足够了

如果有以指定速度转多少圈的需求，才需要使用位置环串联速度环

电流环呢，资助孩子买个电流回传的无刷电机和驱动板好伐？


## 低通滤波优化电机速度环

电机速度环需要对从传感器获得的速度进行滤波，一般使用一阶低通滤波即可： `filter_velocity = a*velocity + (1-a)last_filter_velocity`
 > 一般 `a=0.3`，上一次滤波后的信号权重更大，这样能滤掉信号的上升突变。然后 `a` 越小，过滤效果越强，但是由于从外界获取的新信号权重变小，系统就响应得越慢。并且如果信号没有变，由于 `a + 1-a =1` 实际上滤波后的信号也没有变 

## $\displaystyle FOC$ 无刷电机控制

有刷因为换向点有限只能做到有限细分的力矩控制。转子和磁链的角度不是恒定 90 度的，而是依据电刷细分数在 90 度附近周期性波动。有点类似 6 步方波控制。因为电刷的存在，其长期高可靠运行时不行的

这只是有刷不再大规模使用的一个原因，并不能排除掉直流有刷电机从原理上的优秀的控制性能。因此对于很多其他的电动机来说，对于其控制策珞的研究，就是要向直流有刷电机看齐

实际上 $\displaystyle FOC$ 的根本目的，就是利用磁链解耦把无刷电机“有刷”化，可以通过闭环直接给电流信号来控制力矩、速度、位置，而不用考虑电角度和换向的问题

## $\displaystyle Pure \  Pursuit$ 纯跟踪算法

三二一上链接
-  [PythonAutomatedDriving/README.md at main · muziing/PythonAutomatedDriving (github.com)](https://github.com/muziing/PythonAutomatedDriving/blob/main/README.md)
-  [ApolloAuto/apollo: An open autonomous driving platform (github.com)](https://github.com/ApolloAuto/apollo)
-  [Robotics | 自动驾驶相关各类算法实现及原理分析 (fudanglp.github.io)](https://fudanglp.github.io/Robotics/)
-  [Pure Pursuit 算法 | Robotics (fudanglp.github.io)](https://fudanglp.github.io/Robotics/doc/PathTracking/Pure_Pursuit.html)
- http://www.uml.org.cn/ai/2020060621.asp?artid=22816
- https://blog.csdn.net/adamshan/article/details/80555174

## $\displaystyle MPU 6050$ 解算

2 阶低通滤波加上卡尔曼滤波已经被很多人证明是最好 MPU 控制滤波算法
-  [实现了一个融合加速度和角速度的就已经差不多解决零漂现象的开源仓库](https://github.com/woshinideba1425/Mpu6050-KalmenFilter)

`MPU 6050` 姿态解算有两种方案
1. DMP 库
	1. 硬件加速，不占用外围设备计算资源
	2. 输出四元数
	3. 但是 dmp 不能融合磁力计的数据解算 yaw 轴
2. 用陀螺仪和加速度计的原始数据 raw data 进行姿态解算
	1. 陀螺仪积分不准确
	2. 加速度计易受高频噪声影响

所以姿态解算的方案使用
1. 卡尔曼滤波
2. 互补滤波
3. DMP 四元数

