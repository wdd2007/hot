=========================
Linux Low Level Mechenism
=========================

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::

tcp read/write
==============

read
----

both block and non-block socket will return immediately if there is any data in 
recv buffer without waiting for recv buffer to be full.

when no data in recv buffer

- block

  will return till any data in recv buffer

- non-block

  will return -1 right now with errno=EAGAIN or EWOULDBLOCK


write
-----

- block

  return only when send buffer is enough to hold all the data

- non-block

  return how much send buffer can hold right now with errno=EAGAIN or EWOULDBLOCK


socket
======

Each tcp socket will have a dedicated send buffer and a recv buffer.

To start with linux will use the default values of read and write buffer for each connection. 

As the number of connection increases , these buffers will be reduced [at most till the specified min value] 

::

            net.core.wmem_max = 131071

            net.core.wmem_default = 126976

            net.ipv4.tcp_mem = 378528 504704  757056
                               ------ ------  ------
                               min    default max

            net.ipv4.tcp_wmem = 4096 16384 4194304

            net.ipv4.tcp_rmem = 4096 87380 4194304


tcp handshake
=============

::


        client                  server
          |                       |
          | syn(n)                | r u ready?
          |---------------------->|
          |                       |
          | SYN_SENT              | 
          |                       |
          |     syn  ack=n+1      | i'm ready, r u read?
          |<----------------------|
          |                       | SYN_RCVD (potential syn flood, half-open connection
          |                       |           server maintains an unconnected queue)
          |                       |
          | ack                   | i'm ready
          |---------------------->|
          |                       |
          | ESTABLISHED           | ESTABLISHED
          |                       |

        client                  server
          |                       |
          | fin seq(n)            | i'll close
          |---------------------->|
          | FIN_WAIT_1            |
          |                       |
          |          ack=n+1      | ok, you close
          |<----------------------|
          | FIN_WAIT_2            | CLOSE_WAIT
          |                       |
          |             fin       | i'll close
          |<----------------------|
          |                       | LAST_ACK
          |                       |
          | ack                   | ok, you close
          |---------------------->|
          | TIME_WAIT             | CLOSED
          |                       |
