=========================
GroupCache
=========================

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::

Features
========

- namespace

  Group

- easy deloyment

  both client and server(peer)

- thundering herd avoidance

- no CAS/ttl/inc/dec

- auto handles 'super hot' items

  auto mirror(duplicate entry)

  - The MainCache is the cache for items that this peer is the owner for

  - The HotCache is the cache for items that seem popular enough to replicate to 
    this node, even though it's not the owner
