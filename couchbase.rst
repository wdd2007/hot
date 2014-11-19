=========
couchbase
=========

:Author: Gao Peng <funky.gao@gmail.com>
.. contents:: Table Of Contents
.. section-numbering::

Server
======

Architecture
############

Each server node contains

- TAP replicator

  responsible to handle vBucket migration as well as vBucket replication from active server to replica server.

- data server daemon

  - written in c/c++

  - handle get/set/delete request from client

- mgmt server daemon

  - written in erlang

  - handle the query traffic from client

  - manage the configuration and co-ordinate the other member nodes in the cluster
    - heartbeat
      A watchdog process periodically communicates with all member nodes within the cluster

    - process monitor
      Monitors execution of the local data manager, restarting failed processes and provide status info to heartbeat module

    - configuration manager
      Each node shares a cluster-wide configuration: vBucket map. This manager pull this config from other member
      nodes at bootup time.
      Within a cluster, one node's mgmt server will be elected as the leader.
      When a machine in the cluster has crashed, the leader will detect that and notify member machines in the cluster that all vBuckets hosted in the crashed machine is dead.  After getting this signal, machines hosting the corresponding vBucket replica will set the vBucket status as “active”.  The vBucket/server map is updated and eventually propagated to the client lib.  
      If the leader node crashes, a new leader will be elected from surviving members in the cluster.
      The crashed machine, after reboot can rejoin the cluster.  At this moment, all the data it stores previously will be completely discard and the machine will be treated as a brand new empty machine.


Storage
#######

One file per vBucket.

Data are written to this file in append-only mode.


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


Replica
=======

Currently Couchbase supports one active vBucket zero or more standby replicas hosted in other machines.  

Curremtly the standby server are idle and not serving any client request.  

In future version of Couchbase, the standby replica will be able to serve read request.

Rebalance
=========

The actual data transfer happens directly between the origination node to the destination node.

When new machines are added into the clusters, all existing machines will migrate some portion of its vBucket to the new machines.  

There is no single bottleneck in the cluster.


::

    orchestrator kicks off the process of rebalancing between 2 nodes(src/dest)

    dest node starts up a proces 'ebucketmigrator' which opens a TAP connection to src node

    as soon as all data in src node is sent to dest, src node initiate the switchover process

    While each vbucket is being moved, client access (reads and writes) are still going to the original location. 

    Once the data is all copied over, an atomic switchover happens where the original location says 'I'm no longer the master for this vbucket' and sends a 'token' over to the newly created vbucket saying 'you are'. The original vbucket transitions from active to dead, and the new one transitions from pending to active. Smart clients and Moxi are updated with a new vbucket map to know that this took place and subsequent data requests are directed at the new location
