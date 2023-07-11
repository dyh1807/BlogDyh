---
title: Computer-Network-Final
date: 2023-06-25 22:44:33
tags:
- Networks
- Revision
---

> 用以期末复习临时抱佛脚，以后半期内容为主

## 网络传输

网络层及之下：实现主机到主机的通信，提供无连接的、最大努力交付的数据报传输服务。

而需求不仅仅是数据传输：为了支持应用，主机端还需要实现多进程、确保消息成功传输、消息按序传输、流量控制、拥塞控制等。为此需要传输层对端到端传输做出控制。

传输层：提供应用进程间的端到端通信。

> 一般而言，端系统才有传输层，网络节点仅仅用到下面3层。  
> 特例：NAT

UDP支持主机多进程，而其它的各种功能主要由TCP支持。

端口：进程的标识。16bits整数。($2^{16}=65536$)

在应用进程看来，只要把报文交付给对应的某个端口，剩余的传输工作就可以由传输层协议自动完成。

UDP标识：<主机ip，port>  (目的端口号和目的主机IP)
TCP标识：<source_port, source_ip, target_port, target_ip>

## UDP

提供端到端的尽力而为的数据报传输服务。

提供的功能:
- 多路复用/分解:端口功能
- 差错检测

Checksum的计算方法：数据划分为16bits相加（不足的部分后续补0），将所有的16bit值累加到一个32bit的值中；将32bit值的高16bit与低16bit相加到一个新的32bit值中，若新的32bit值大于0Xffff, 再将新值的高16bit与低16bit相加；将计算所得的16bit值按位取反，即得到checksum值，存入数据的checksum字段即可

注意UDP计算Checksum的时候增加了“伪首部”，“伪首部”并不会真正发送。

伪首部包括12字节：Source IP，Target IP，0，17，UDP length

## TCP

TCP计算Checksum也需要“伪首部”：同样为12 Bytes.

对于TCP,伪首部包括12字节：Source IP，Target IP，0，6，TCP length

MSS:最大报文段长度。
- 如果TCP报文段长度太短，网络利用率低（都在传输头部信息了）；如果太长，在IP层又需要分段，分段后还需要重新装配，导致开销增大。

TIME-WAIT阶段等待2MSL：（2倍数最长报文段寿命）

选择重传SR：通信双方需要确认好是否使用SACK字段。

....