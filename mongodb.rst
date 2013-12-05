=========================
mongodb internal
=========================

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::

used tcmalloc

BackgroundJob starting: ClientCursorMonitor
BackgroundJob starting: TTLMonitor
BackgroundJob starting: PeriodicTask::Runner



Flushing
========

journal
-------

100ms

background
----------

msync every 60s

customize with syncDelay option

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

- capped collection

  fixed size, designed for logging storage; no _id index by default

  can't delete individual documents

- Databases and collections are created only when documents are first inserted

  use 'strict mode' to disable this feature

- no joins


Limitations
===========

- BSON documents limited to 16M in size

ObjectID
========


BSON
====
all documents are stored on disk as BSON, and all queries and commands are specified using BSON documents

All documents must be serialized to BSON before being sent to mongod, which is done by driver

Indexing
========

Save as B-tree in mysql, so working with indexes in MySQL can expect similar behavior in MongoDB.

> db.numbers.find({num:{"$gt":1, "$lt":200000}}).explain()

If you have a unique index on slug, you’ll need to insert your product document using safe mode so that you’ll know if the insert fails. That way, you can retry with a different slug if necessary. 


Write
============

All writes, by default, are fire-and-forget, which means that these writes are sent across a TCP socket without requiring a database response. 
If users want a response, they can issue a write using a special safe mode provided by all drivers. 
This forces a response, ensuring that the write has been received by the server with no errors. 
Safe mode is configurable; it can also be used to block until a write has been replicated to some number of servers.

In MongoDB v2.0, journaling is enabled by default. With journaling, every write is committed to an append-only log. 


Preallocation
=============

to ensure that as much data as possible will be stored contiguously.


mmap
====



php extention 1.2.0
===================

::

        $uid = 1;
        $u = new UserAccountModel($uid);
        $u->getAccount();


        c->s    3 way tcp handshake
        c->s    admin.$cmd(isMaster)
        c<-s    reply, maxBsonObjectSize maxMessageSizeBytes localTime
        c->s    admin.$cmd(ping)
        c<-s    ok
        c->s    findOne(royal_1.user, guid=1)
        c<-s    reply
        c->s    FIN and 4 way closehand


    php_mongo_get_reply
        get_header

    connectUtil
        mongo_util_pool_get


Election
========

It may take 10-30 seconds for the members of a replica set to declare a primary inaccessible. This triggers an election. During the election, the cluster is unavailable for writes.

The election itself may take another 10-30 seconds.
