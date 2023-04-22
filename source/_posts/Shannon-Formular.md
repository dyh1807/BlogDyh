---
title: Shannon Formula
date: 2023-04-22 21:48:14
tags: 
- Computer Network
- Shannon's formula
---
 
香农公式：

\[
C = W\log_{2}{\left(1+S/N\right)}
\]

一直对C的认识不是很清楚，只知道其单位是bps（当W的单位是 Hz 时），指最大可靠传输速率。但“最大可靠传输速率”代表的是什么呢？是数据的 bps 还是 码元的 bps？

今天看到谢希仁《计算机网络》中提到：可以通过让码元携带更多的信息提高信息传输速度。这才理解到香农公式中的 C 指的是码元的 bps。