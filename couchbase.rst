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

