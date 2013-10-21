=========================
TCP/IP
=========================

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::


============
路由器队列经常要临时存储数据包直到它们可以转发出去才开始flush，当路由器队列空间不够时，它们就开始丢弃数据包，导致重传，进而导致重复和乱序传递数据包。

udp: datagram
tcp: segment

UDP在IP基础上增加了2个服务

- 数据校验

  IP的校验只是IP的报头部分

- 端口

TCP在IP的服务上增加了4个服务

- 为segment提供了校验位

  确保到达目的地的数据不会在网络传输时被破坏

- 为每个segment分配一个序列号

  保证有序

- 提供了确认和重传机制

  保证每个segment会到达目的地

- 滑动窗口

  TCP每一方都维护一个recv window，该窗口的范围是将要接收对方的数据的序列号

  超出窗口的带序列号的数据被丢弃

私有IP

- 192.168.0.0 ~ 192.168.255.255

- 172.16.0.0 ~ 172.31.255.255

- 10.0.0.0 ~ 10.255.255.255
