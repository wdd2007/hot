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

  [metrics id] [base timestamp] [tagid tagvalue] [tagid tagvalue...]
  ------------ ---------------- ------ --------- -------------------
  24b          32b              24b    24b

- column qualifier

  t:[time delta] [value type] [reserved]
    ------------ ------------ ----------
    12b          1b           3b

  value type can only be float or int

