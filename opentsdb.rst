=========================
OpenTSDB
=========================

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::


Schema
======

tsdb
----

- rowkey

::

  base timestamp = 60m

  [metrics id] [base timestamp] [tagid tagvalue] [tagid tagvalue...]
  ------------ ---------------- ------ --------- -------------------
  24b          32b              24b    24b          24b     24b
   |            |                |      |            |       |
    -----------                   ------              -------
       56b                          48b                 48b

- column qualifier

::
  
  time delta in seconds offset base timestamp

  t:[time delta] [value type] [reserved]
    ------------ ------------ ----------
    12b          1b           3b

  value type can only be float or int

  value is 8 bytes


Usage
=====

::

    metricName timestamp value [tag k/v]
    ----------           -----
    mkmetric             int64 or double


Enhancement
===========

graph
-----

src/graph/Plot.java, which generate gnuplot script on time
