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
     |            |            |
      -------------------------
            16b

  value type can only be float or int

  value is 8 bytes


tsdb-uid
--------

::

    hbase(main):004:0> scan 'tsdb-uid'
    ROW                          COLUMN+CELL
    \x00                        column=id:metrics, timestamp=1380113778607, value=\x00\x00\x00\x00\x00\x00\x00"
    \x00                        column=id:tagk, timestamp=1380113861107, value=\x00\x00\x00\x00\x00\x00\x00\x07
    \x00                        column=id:tagv, timestamp=1380113861114, value=\x00\x00\x00\x00\x00\x00\x00\x1D
    \x00\x00\x01                column=name:metrics, timestamp=1380113390180, value=mysql.bytes_received
    \x00\x00\x01                column=name:tagk, timestamp=1380113860883, value=host
    \x00\x00\x01                column=name:tagv, timestamp=1380113860887, value=gp
    \x00\x00\x02                column=name:metrics, timestamp=1380113390188, value=mysql.bytes_sent
    \x00\x00\x02                column=name:tagk, timestamp=1380113860895, value=type
    \x00\x00\x02                column=name:tagv, timestamp=1380113860901, value=telnet
    \x00\x00\x03                column=name:metrics, timestamp=1380113778473, value=tsd.compaction.count
    \x00\x00\x03                column=name:tagk, timestamp=1380113860928, value=cache
    \x00\x00\x03                column=name:tagv, timestamp=1380113860907, value=http
    \x00\x00\x04                column=name:metrics, timestamp=1380113778478, value=tsd.compaction.deletes
    \x00\x00\x04                column=name:tagk, timestamp=1380113860968, value=kind
    \x00\x00\x04                column=name:tagv, timestamp=1380113860912, value=all
    \x00\x00\x05                column=name:metrics, timestamp=1380113778482, value=tsd.compaction.errors
    \x00\x00\x05                column=name:tagk, timestamp=1380113860996, value=class
    \x00\x00\x05                column=name:tagv, timestamp=1380113860919, value=graph
    \x00\x00\x06                column=name:metrics, timestamp=1380113778486, value=tsd.compaction.queue.size
    \x00\x00\x06                column=name:tagk, timestamp=1380113861026, value=method
    \x00\x00\x06                column=name:tagv, timestamp=1380113860923, value=gnuplot
    

Limitations
===========

You can see from tsdb-uid, each row key is 3 bytes(24bit). Each metics/tagk/tagv have their 
own UID spaces.

- metrics count

  2 ** 24 = 16,777,216

- tag names

  2 ** 24 = 16,777,216

- tag values

  2 ** 24 = 16,777,216

Usage
=====

Metrics need to be registered before you can start storing data points for them.

New tags are automatically registered whenever they're used for the first time.

::

    ./tsdb mkmetric mysql.bytes_received mysql.bytes_sent


::

    metricName timestamp value [tag k/v]
    ----------           -----
    mkmetric             int64 or double


Enhancement
===========

graph
-----

src/graph/Plot.java, which generate gnuplot script on time
