---
title: My Projects
date: 2023-06-05 20:40:20
tags:
- Project
categories:
- Project
---

> 不同于去年秋季不算正式的“实习”，今日第一次收到“实习面试”的通知，老师提示了可以介绍已经做过的项目、课程学到的内容。考虑到对于项目的介绍在以后找工作、面试是也是可能用到的，这里记录下已经做过的几个“项目”的基本内容。

## Contents

- CALab
- AICS Lab
- Machine Learning Project
- Gomoku
- Computer Network Lab
- Compiler Lab(LLVM)

## CALab

### Project Introduction

1. 基本的单周期CPU，支持运算指令、访存指令、分支指令。
2. 基于单周期CPU，改造成流水线CPU，通过阻塞或者前递机制解决流水线冲突
3. 添加中断和异常支持
4. 添加AXI总线支持
5. 添加TLB、MMU支持

### 可能应对的问题

1. CPU的设计流程
2. 流水线的实现逻辑
3. 冲突有几种，已经应该如何解决冲突
4. 乘法器和除法器逻辑如何实现？
5. 中断和异常有什么区别？
6. 有哪些经典的中断或者异常？
7. 什么是“精确异常”？
8. 为什么需要AXI总线？
9.  TLB、MMU是什么，完成了什么工作，如何完成工作？

### Answer

1. 围绕“一条指令是如何工作的”进行设计。指令的执行可以拆解为“取指令、译码、执行、访存、写回”。单周期CPU中，主要采用组合逻辑在一个周期中中完成全部工作。

2. 根据指令的执行逻辑，将一条指令的工作划分为5级流水线，后一级流水线是否可以接收上一级流水线取决于上一级流水线是否完成了工作以及本机流水线是否允许新的指令进入。在时钟信号的每个上升沿，判断指令是否可以达到下一级流水线。

3. 指令相关：包括数据相关（需要的数据没到达）、结构相关（必要的部件数量有限）、控制相关（分支指令冲突访问Program Counter）

4. Booth算法、Wallace Tree、类似笔算除法的迭代除法器

5. 一种观点是，从CPU设计的角度看，中断是一种特殊的异常；二者的处理逻辑是类似的。

6. 中断：定时器时钟中断；异常：指令不存在、地址未对齐、

7. 指处理异常的过程仿佛“不存在”。为此，处理前需要做必要的保存，处理后需要恢复处理异常前的PC。

8. CPU不可能独立存在。在一个计算机系统中，需要和其它部分实现交互。实际情况下，采用的交互接口为总线接口。因此，这里实现了一个AXI总线接口，以便提供更加同样的交互。

9. TLB(Translate Lookaside Buffer)存储页表的Cache，MMU负责完成虚实地址转换。虚实地址转换：对某个虚拟地址访问，MMU先查看TLB,如果对应的页表在TLB内，就直接得到了实地址；否则，触发TLB异常指令（TLB充填、store页无效等）。

## AICS Lab

### Project Introduction

1. 三层MLP实现
- Python+Numpy实现
-  - forward 计算$y=f(x; w, b)$
-  - backward 计算 $\nabla_{x}{y}$, $\nabla_{w}{y}$, $\nabla_{b}{y}$, 返回$\nabla_{x}{y}$  
- 调用 pycnml实现  

2. 卷积神经网络VGG19 的实现
- 基于VGG19实现图像分类(Python+Numpy，将卷积层、池化层、扁平操作、全连接层、ReLU操作、Softmax操作封装为类)  
- 基于VGG19实现图像分类(使用pycnml库完成同样给功能)  
- 基于VGG19实现非实时图像风格迁移
- - 原理：利用一个（已经训练好参数的）VGG19网络，先后提取风格特征和内容特征，然后进入迭代过程，每次迭代先提取transfer_image的推理信息，然后依据需要利用的feature卷积层和content卷积层，做backward操作（每层都backward到input为止）并相加，从而得到 total dloss (for both content and style),最后（一次迭代的最后）利用adam优化器做优化。
- - 参考：AICS手写笔记`P31`.

3.  Tensorflow v1 智能编程框架使用经验
- 使用tensorflow搭建VGG19，并在cpu、dlp两种平台使用。
- 实时风格迁移预测（分别在cpu和dlp上推理）。
- - 稍微了解了实时风格迁移是怎样的，并没有写出整个算法是怎样的，而是利用已经存在的Pb文件中的结点作为输入和输出。（体会到tf的方便之处：不用写整个算法就可以做推理）
- 实时风格迁移训练。
- - 首先构建实时风格迁移对应的网络（卷积层、残差块、转置卷积组成的网络）。
- - 然后定义损失函数
- - 最后，用tensorflow描述实时迁移的执行过程（声明），然后循环迭代执行训练。因为tf自动实现了反向传播，因此用起来非常方便（不需要再手动公式求导后描述成backward代码）。
- 自定义算子并集成tensorflow (PowerDifference 算子开发)
- - 修改pb模型文件（pb->pbtxt->修改后的pbtxt->修改后的pb）
- - Numpy 实现 `power_diff_NumPy(intput_x,  input_y, input_z)` 算子（函数）
- - C++ 实现算子（PowerDifferenceOP类，继承于 OpKernel），然后注册算子（所谓注册，指的是告知tensorflow自定义了例如"PowerDifference"这样的操作），最后编译算子（使用Bazel）得到新的tensorflow（此时支持了C++实现的算子PowerDifference）

> 因为都是用到的时候才看一下，所以具体的 tf 细节已经不记得了，只记得主要的特点是：先描述计算（声明），然后建立会话（session）、在会话中填充数据并完成计算、关闭会话。

4.  BangC 算子开发
- BCL/BPL 算子开发
- 性能优化实验

> 基本看着手册、实验指导做的，其它忘了

5. 综合实验
   
> 没仔细做，差不多基本没做，拿了YOLOv3的60分档。

### 可能应对的问题

1. 实时风格迁移和非实时风格迁移有什么区别？
2. 。。

### Answer

1. 一个快，效果没那么好；一个慢，效果可能好一些。

2. 。。


## Machine Learning Project

## Gomoku

## Computer Network Lab

## Compiler Lab(LLVM)
