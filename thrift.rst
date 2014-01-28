======
Thrift
======

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::

Stack
=====

::


        Server(single threaded, event driver, etc)
           |
        Processor(generated)
           |
        Protocol(json, compact, binary, etc)
           |
        Transport(tcp, http, etc)

Types
=====

supports c style typedef

::

    typedef i32 MyInteger


- bool

- binary

  []byte

- byte

- i16

- i32

- i64

- double

- string

- enum

  ::

        enum OpType {
            SET,
            GET = 2,
            DELETE,
        }

- list<t>

  - ordered

  - may contain duplicates

- set<t>

  - unordered

  - unique

- map<t1, t2>

- struct

  - can be nested

  - can't inherit

- exception

- const


Services
========

May extend other service.
