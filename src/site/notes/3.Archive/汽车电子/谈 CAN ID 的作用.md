---
{"dg-publish":true,"dg-path":"汽车电子/谈 CAN ID 的作用.md","permalink":"/汽车电子/谈 CAN ID 的作用/","created":"2019-09-27T09:22:51.000+08:00","updated":"2024-11-19T11:21:17.877+08:00"}
---

#BDStar #AutoSAR 

帧 ID 是什么？通俗讲是 CAN 的一种“地址”。CAN 有个特点是竞争机制，帧 ID 越小越有占用总线资源的权利，越会优先发送。举个例子，灯光信号帧 ID 0x555，发动机温度传感器帧 ID 0x111，那么当两个信号同时发出时，发动机的信号会优先发送，灯光在后面排队。通常在一个 CAN 系统中，不同的设备，发出 CAN 信号的帧 ID 都是不一样的，或者说，CAN 信号的每个帧 ID 都有一个固定的用途。如果一组 CAN 信号的帧 ID，它们的用途都被确定下来，并在一个文档中得到了解释，那么我们管这个叫 CAN 总线的应用层协议，或者高层协议。常见的有 ISO15765，SAE J1939，CANopen（电机和挖沟机控制器用）。在车辆行业中，如果对车辆 CAN 总线上的每个帧 ID 及每个帧数据都做出了标准的解释，形成了文件的话，我们叫这个文件为 DBC 文件。设计 CAN 节点时我们应该注意，不要给不同的节点设置相同的帧 ID，这样会导致仲裁错误，进而导致接收不到某个 CAN 节点的数据。