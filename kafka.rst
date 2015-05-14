=========================
kafka vs nsq
=========================

:Author: Gao Peng <funky.gao@gmail.com>
.. contents:: Table Of Contents
.. section-numbering::

Terms
=====

Kafka
#####

- topic 
- partition
- offset
- consumer group

NSQ
###

- topic
- channel


API
===

Kafka
#####

- producer
  produder need to know about partition scheme

  produer::send(topic, partition, message)

- consumer
  consumer:iterate<message>
