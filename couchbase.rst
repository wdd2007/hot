=========
couchbase
=========

:Author: Gao Peng <funky.gao@gmail.com>
.. contents:: Table Of Contents
.. section-numbering::

Server
======

Metadata
########

By default, all documents contain metadata that is provided by the Couchbase Server. 

- CAS value
  optimistic version 

- TTL
  by default, each document has indefinite life span

- Flags
  used by a Couchbase client library to perform additional processing of a document


Client
======

Smart client.

::

    curl http://localhost:8091/pools/default/buckets/default


Rebalance
=========

::

    orchestrator kicks off the process of rebalancing between 2 nodes(src/dest)

    dest node starts up a proces 'ebucketmigrator' which opens a TAP connection to src node

    as soon as all data in src node is sent to dest, src node initiate the switchover process

    While each vbucket is being moved, client access (reads and writes) are still going to the original location. 

    Once the data is all copied over, an atomic switchover happens where the original location says 'I'm no longer the master for this vbucket' and sends a 'token' over to the newly created vbucket saying 'you are'. The original vbucket transitions from active to dead, and the new one transitions from pending to active. Smart clients and Moxi are updated with a new vbucket map to know that this took place and subsequent data requests are directed at the new location
