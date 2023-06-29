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

参考[Christopher F Clark's Answer](https://www.quora.com/What-is-the-relation-between-a-process-and-a-core-Is-there-a-difference-between-the-core-s-threads-and-process-s-threads)

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

## QX

To be continue...

## AX

To be continue...
