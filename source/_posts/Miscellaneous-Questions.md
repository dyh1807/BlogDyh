---
title: Miscellaneous Questions
date: 2023-06-20 12:54:38
tags:
---
> 记录各类杂项问题

## Q1

线程和进程的区别？

## A1 (Sys)

进程是一个程序的执行，线程是可被调度执行的最小的单位，一个进程中可能有多个线程。

从OS的角度看，一个CPU核心 core 执行一个 thread；实际上，一个 core 可能可以同时（物理上）执行多个 thread（这样的能力称为 Hyperthreading），但是从OS的视角看，这就是2个 core 在执行 2个 threads。

一个进程和 core 的数量没有直接的关系。

参考[Christopher F Clark&#39;s Answer](https://www.quora.com/What-is-the-relation-between-a-process-and-a-core-Is-there-a-difference-between-the-core-s-threads-and-process-s-threads)

## Q2 (AI)

什么是预训练？它和正常的训练有什么区别？为什么预训练有助于改善模型的效果？

## A2

我初次了解到预训练来自于VGG。VGG的预训练做法是：先训练一个11层的 NN；训练更深的NN时B，使用A的部分层替换B的部分层。

查看西瓜书和花书，预训练可以分为2阶段: pre-train + fine-tune. 可以采用一种 `greedy layer-wise pretraining` 的方法，逐层训练网络(pre-train)；最后再在整个网络上做训练(fine-tune)。

> 在我看来，预训练的重点是调整了每个参数的训练次序。(虽然训练次数也可能和正常的训练有所调整)

> 此外，2006年 Geoffrey Hinton 的 deep brief network 采用了预训练 `greedy layer-wise pretraining`，被认为是第三次神经网络热潮的突破点。

关于预训练（pretraining）的定义，来自花书：一种策略，在目标模型工作在目标任务之前，在简单模型上做简单的任务。

更加详细的说明：有时候，直接在特定 task 上训练 model 过于野心了(如果模型很复杂，那么这可能很难做到)。

> 因此，预训练是一种优化策略(Optimization Strategies)

除了加快了训练速度以外，实验证明，经过预训练的模型就是能取得比未预训练更好的效果。(在众多的已知任务上)

## Q3 (AI)

ResNet 中的 shortcut 的目的、作用？(来源：AICS-design internship interview)

## A3

todo

## Q4 (AICS)

AI框架设计中，静态图和动态图有什么差异？

## A4

|     | 静态图 | 动态图 |
| --- | ----- | ------ |
| 差异 | 需要先构建再运行 | 一边运行一边构建|
| 优点 |在运行前可以对图结构进行优化，比如常数折叠、算子融合等，可以获得更快的前向运算速度 | 可以在搭建网络的时候看见变量的值，便于检查 |
| 缺点 | 只有在计算图运行起来之后，才能看到变量的值，像TensorFlow1.x中的session.run那样。 | 前向运算不好优化，因为根本不知道下一步运算要算什么。|

## Q5 (AICS)

AI框架中的一个“算子”指的是什么？

## A5

算子(Operator)指的是一种操作，可以视为一个从输入到输出的映射。卷积、池化等操作均可以被抽象为算子。

算子可能有不同的实现，所谓“高性能算子”就是高性能的算子实现（C++实现、Python实现均不同，寒武纪BCL与BPL、NVIDIA的CUDA均提供算子实现的“智能编程语言”等级描述）。

## QX

To be continue...

## AX

To be continue...
