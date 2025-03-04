---
{"dg-publish":true,"dg-path":"汽车电子/IP 地址类别备忘录.md","permalink":"/汽车电子/IP 地址类别备忘录/","created":"2022-06-16T15:45:35.000+08:00","updated":"2024-11-19T14:26:05.000+08:00"}
---

#Ofilm #ETH

在以太网中，MAC 地址用来区分同一子网中的不同设备，IP 地址用来区分目标主机位于哪一个子网。IP 地址的重要性不言而喻，下面我们就来谈谈 IP 地址有哪些类别：

# 基于传统分类的 IP 地址

IPv4 地址按照其前几位的特征，分为五类：A 类、B 类、C 类、D 类、E 类

## A 类地址

- 范围
	- 起始地址：0.0.0.0
	- 结束地址：127.255.255.255
	- 网络号范围：0.0.0.0 - 127.0.0.0
- 特征
	- 第一个字节（8 位）的最高位固定为 0
	- 地址范围：1.0.0.0 至 126.0.0.0 （注意：127.x.x.x 是保留地址，用作环回测试）
- 子网划分
	- 网络部分：8 位（1 个字节）
	- 主机部分：24 位（3 个字节）
- 用途
	- 为拥有大量主机的大型网络设计（如大企业、机构）
- 主机数量
	- 单个网络可容纳 2^24 - 2 台主机（除去网络号和广播地址）

## B 类地址

- 范围
	- 起始地址：128.0.0.0
	- 结束地址：191.255.255.255
	- 网络号范围：128.0.0.0 - 191.0.0.0
- 特征
	- 前两个字节（16 位）的最高位固定为 10
	- 地址范围：128.0.0.0 至 191.255.255.255
- 子网划分
	- 网络部分：16 位（2 个字节）
	- 主机部分：16 位（2 个字节）
- 用途
	- 为中型网络设计（如中型公司和大学）
- 主机数量
	- 单个网络内可容纳 2^16 - 2 台主机

## C 类地址

- 范围
	- 起始地址：192.0.0.0
	- 结束地址：223.255.255.255
	- 网络号范围：192.0.0.0 - 223.255.255.0
- 特征
	- 前三个字节（24 位）的最高位固定位 110
	- 地址范围：192.0.0.0 至 223.255.255.255
- 子网划分
	- 网络部分：24 位（3 个字节）
	- 主机部分：8 位（1 个字节）
- 用途
	- 为小型网络设计（如小公司或家庭网络）
- 主机数量
	- 单个网络可容纳 2^8 - 2 台主机

## D 类地址

- 范围
	- 起始地址：224.0.0.0
	- 结束地址：239.255.255.255
- 特征
	- 前四位固定为 1110
	- 不划分为网络部分和主机部分
- 用途
	- 用于多播通信，即将数据同时发送到多个目标（如视频会议、流媒体）

## E 类地址

- 范围
	- 地址地址：240.0.0.0
	- 结束地址：255.255.255.255
- 特征
	- 前四位固定为 1111
- 用途
	- 保留地址，实验性用途
	- 当前未用于正式网络通信

# 特殊 IP 地址

- 环回地址
	- 范围：127.0.0.0 - 127.255.255.255
	- 用途：本地测试通信（如 127.0.0.1，表示本机地址）
- 广播地址
	- 范围：网络号后全为 1（如 192.168.1.255）
	- 用途：向网络中的所有设备发送消息
- 私有地址
	- 用于访问内部网络，不可直接访问互联网：
		- A 类：10.0.0.0 - 10.255.255.255
		- B 类：172.16.0.0 - 172.31.255.255
		- C 类：192.168.0.0 - 192.168.255.255
- 自动私有地址（APIPA）
	- 范围：169.254.0.0 - 169.254.255.255
	- 用途：当 DHCP 无法分配地址时，自动分配的临时地址

# 无类别域间路由（CIDR）

- 传统的 A/B/C 类地址划分在实际中可能浪费地址资源
- CIDR 引入了前缀表示法，如 192.168.1.0/24，直接用于网络前缀长度来表示网络部分和主机部分的界限
- 优点：
	- 更高效地分配 IP 地址
	- 避免地址资源浪费