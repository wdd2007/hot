=========================
Performance Metrics Cases
=========================

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::


syslog-ng
=========

Our central syslog server, an 8-core server with 12Gb RAM, currently handles 
around 60,000 events per second at peak at ~25% CPU load.


json
====

fastjson
--------

size = 468B

- ser

  2292 ns

- deser

  1499


protobuf
--------

size = 239B

- ser

  3008 ns

- deser

  1694 ns
