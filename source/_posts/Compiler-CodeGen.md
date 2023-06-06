---
title: Compiler-CodeGen
date: 2023-06-06 20:46:41
tags:
- Compiler
categories:
- Compiler
- Learning Notes
---

> 记录下 DragonBook 第八章的学习

## Task for Code Generator
代码生成器的工作：接收 IR，高效产生目标代码。

## 8.1 代码生成器设计中的问题

### 寄存器的使用
分为2个子问题：
1. 寄存器分配：选择出一组将被放在寄存器中的变量
2. 寄存器指派：指定变量将被放在哪个寄存器中

## 8.2 目标语言

### 程序和指令的代价

不同的指令具有不同的代价。

简单的划分：一条指令的代价为： $1 + 访存代价$

## 8.3 目标代码中的地址

> 静态和栈式的内存分配是调用和返回生成代码的两种方式

### 静态分配
静态分配是一种函数调用的方法，指的是把被调用函数`callee`的返回地址存储到其静态区域`callee.staticArea`的方法。

### 栈分配
使用栈记录函数的活动记录。需要寄存器SP指向栈顶。

### 名字的运行时地址
三地址码相对地址+对应的分配策略（静态static 或者 栈式分配）对应的偏移=名字的运行时地址。

## 8.4 基本块和流图

基本块：最大连续三地址指令序列。在基本块最后一条指令前，不会发生跳转、停机。

流图(flow graph)：基本块之间存在先后运行关系，构成流图。

### 后续有用信息

名字`x`在语句（指令）`i`处活跃：存在后续的指令`j`使用了在指令`i`处被赋值的名字`x`

### 循环

称流图中的结点集合`L`是一个循环，如果：  
1. L存在一个循环入口结点（L中，只有入口结点可以存在非L中结点前驱）
2. 每个结点都有一条在L中的路径可以达到入口结点。

## 8.5 基本块的优化

### 基本块的 DAG 表示

> Here it refers to the DAG representation within a basic block

构造DAG有以下的基本原则：
1. for each variable, there is a DAG node w.r.t it's initialize value(in this basic block)
2. ..
3. ..
4. 有的结点称为output node，它们在基本块的出口处活跃。

By DAG, we can do:
1. eliminate local common subexpression
2. eliminate dead code
3. reorder instructions
4. simplify calculation



