=========================
ElasticSearch
=========================

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::

Cluster
============

nodes
-----

====== ==== ==========
master data node type
====== ==== ==========
Y      Y    normal node 
Y      N    coordinator
N      Y    workhorse
N      N    search load balancer
====== ==== ==========


shards and replicas
-------------------

- index.number_of_shards is a one-time setting for an index.

- index.number_of_replicas can be increased or decreased anytime.

