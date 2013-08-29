=========================
mongodb internal
=========================

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::


Scenario
========

- session

- primary data store

- uploads

- log

Story
=====

- TheBusinessInsider use MongoDB as primary data store since 2008.2

- NYT

Feature
=======

- strong consistency

- updates can be applied atomically to individual documents

- Databases and collections are created only when documents are first inserted

  use 'strict mode' to disable this feature


ObjectID
========


Index
=====

Save as B-tree in mysql, so working with indexes in MySQL can expect similar behavior in MongoDB.

> db.numbers.find({num:{"$gt":1, "$lt":200000}}).explain()


Write
============

All writes, by default, are fire-and-forget, which means that these writes are sent across a TCP socket without requiring a database response. 
If users want a response, they can issue a write using a special safe mode provided by all drivers. 
This forces a response, ensuring that the write has been received by the server with no errors. 
Safe mode is configurable; it can also be used to block until a write has been replicated to some number of servers.

In MongoDB v2.0, journaling is enabled by default. With journaling, every write is committed to an append-only log. 


mmap
====


