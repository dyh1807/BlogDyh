---
title: Intermediate-Code-Generation
date: 2023-04-20 22:55:47
tags: Compiler
catagories:
- Learning Notes
- Compiler
---
>  插入图片和Latex公式有待部署

刚刚经过 compiler 课程的第一次小测，本来感觉自己正常学学得差不多了；但现在看来，学的时候陷入了一个误区：认为学的时候看一遍算法是怎样工作的就行了。实际上，只是看一遍算法的工作方式并不会让人懂得其中的“原理”。看算法并不重要，重要的是梳理清楚“要做什么”“为什么这样做”。另一个问题是英语阅读的问题：开学至今，我一直坚持看原版的dragon book,有时候也可以比较快速的浏览；但持续到现在，我感觉语言对于理解内容还是造成了一定的阻碍，不如之后直接阅读听说翻译得比较好的中文。

This post will keep with every key item in Chapter 6: Intermediate-Code Generation. The following part is the key:

1. Purpose of the Chapter
2. How to deal with the problem

## Preface

The logical structure of a compiler:

![插入图片功能还没有部署完毕](./Intermediate-Code-Generation/Screenshot%202023-04-20%20232133.png)

> This is a test to insert figure in post as well.

The front end generate IR, and the back end handle the IR to generate the target language.

A flow of language translation can be:   
source code -> AST -> Three address code -> target code

> For some language's compiler, C could be an intermediate code as well.

To know everything of the chapter, first have a quick view:

## A Quick View

1. Variants of AST
2. Three Address Code
3. Type and Declaration
4. Translation of Expression
5. Type Checking
6. Control Flow
7. 回填(backpatching)
8. switch statement
9. The Intermediate-Code of Process

1 and 2 are IR, which are the target to generate in this chapter. 3-9 are more concrete items.

## Variants of AST

That is: DAG(Directed Acyclic Graph)

### why DAG? ---- for expression representation
AST represent the logical structure of a piece of code. DAG is like AST, but same space in some cases, especially when it comes to expressions.

for instance, DAG saves space for the expression:
$$a+a+a+a+a+a+a+\cdots+a$$
> A node in DAG may have many parent.

### how to get DAG?
Just like AST: by SDD.
> Different from AST, it need to check whether node have in the DAG or not during the process of gereration.

通过在SDD中加入创建结点、叶子的动作，在parse的过程中同步生成DAG.

### 值编码方法
什么是值编码(value number)？即为结点中的一个字段，代表了这个结点在记录结点的数组中的位置.
> 可以理解为数组下标

对一个表达式构造/搜索值编码：  
Input: op, l, r
Output: value number of <op, l, r>   
Method1:
1. Search the Array with <op, l, r>. If succeed, return the value number.  
2. If fail: add a node and return the value number.

Method2(More efficiently):
1. set hash function h
2. calculate(h<op, l, r>) and search the correspond list.

## Three-address code

feature: one operand at most.

Intermediate name may be introduced.

Three address code consist of:  
1. Address 地址，可以是：名字、常量、编译器生成的临时变量
2. Instruction 

三地址码可以有多种表示方式：

### 四元式表示

op, arg1, arg2, result

### 三元式表示

op, arg1, arg2

用 x op y 的位置表示计算结果，而不是显式表示

间接三元式：包含了一个指向三元式的指针的链表。

静态单赋值(SAA): 对相同变量的不同次赋值操作，采用不同的下标。

## 类型和声明

类型的使用有两个重要的作用：类型检查 type checking、翻译时应用 translation application

type checking: 程序运行时，检查程序行为，例如 boolean 运算 && 的 两个成员是否都是 boolean 类型。

translation application: 例如，根据类型，编译器可以确定名字在运行时刻需要多大的空间。

### 类型表达式

type expression, 表示了 type 的 structure.

可以是基本的(int, float...)，也可以是复合构造出来的(array, struct, ...)

type expression can be expressed by DAG

### 类型等价

有3种“类型等价”：
1. same basic type
2. same constructed type
3. one type is the name another type expression
4. 
## Status

to be continue...

