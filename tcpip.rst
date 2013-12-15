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


TCP
===
是个流协议，没有内在的消息和消息边界概念。

读TCP数据时永远不知道read会返回多少字节

send返回，只是拷贝到TCP协议栈里的send buffer

TCP应答采用的是带时延的ACK， 比如收到数据后先看有没有应用层数据，有的话就一起发出去
没有就等待200ms，仍然没有应用层数据，就单独发ACK

To get maximal throughput it is critical to use optimal TCP send and receive socket buffer sizes for the link you are using. If the buffers are too small, the TCP congestion window will never fully open up. If the receiver buffers are too large, TCP flow control breaks and the sender can overrun the receiver, which will cause the TCP window to shut down. This is likely to happen if the sending host is faster than the receiving host. 

optimal buffer size = bandwidth * RTT

但从Linux2.6之后，出现了TCP autotuning，就不需要手工设置send and recv buffer了

    建立握手需要 1.5 RTT

    Client                      Server
      | SYN(MSS,WIN,SEQ                                     |
      |-----------------------------------------------------|
      |                                                     |
      |-----------------------------------------------------|
      |                                                     |
      |-----------------------------------------------------|

TCP无法将丢失的连接立即通知应用程序，如果不发送数据，应用程序可能永远都无法发现连接的丢失

SO_REUSEADDR是因为TIME_WAIT 2MSL

UDP
===
即使在同一台机器上运行C/S，UDP也会发生丢包，这是因为缓冲区空间不足造成的

UDP的报文长度最大可以达到64 kb，但是当报文过大时，稳定性会大大减弱。这是因为当报文过大时会被分割，使得每个分割块（翻译可能有误差，原文是fragmentation）的长度小于MTU，然后分别发送，并在接收方重新组合（reassemble），但是如果其中一个报文丢失，那么其他已收到的报文都无法返回给程序，也就无法得到完整的数据了。 

Tuning
======

net.core.r(w)mem_max = 127K
--------------------------

Max OS receive/send buffer size for all types of connections.

net.core.r(w)mem_default = 126K
-------------------------------

Default OS receive/send buffer size for all types of connections.

TCP Autotuning
--------------

net.ipv4.tcp_r(w)mem = 4K 85K 4M
#################################

- 每个socket的最小缓冲区

- 每个socket的默认缓冲区

- 每个socket的最大缓冲区


tcp seq
#######

::

        client                  server
        ------                  ------
                syn(C) ack(0)
                ---------->

                syn(S) ack(C+1)
                <---------- 
                
                syn(C+1) ack(S+1)
                ----------->

                3-way handshake done! and ready for data transfer

                push(syn(C+1:C+1+L) ack(S+1) segmentLen(L))
                ----------->

                syn(S+1) ack(C+1+L) segmentLen(0)
                <-----------

                之后，c->s里的ack值一直是S+1，而s->c里的syn也一直是S+1，syn/ack的值不再执行加一操作
                因为建立握手时的SYN要占用一个序列号
