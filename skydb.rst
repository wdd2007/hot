=========================
skydb(behavior database)
=========================

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::


Overview
========

actions + state over time.

Each user would have a hash in Sky.

storage
-------

Each server has {numCPU} servlets.

::

    {
        user: {
            // permanent properties
            gender: F   
            birth: 1986-10

            // transient properties because their key is time based
            actions: {
                signup: {
                }

                checkout: {
                    1387030582: {
                        purchase_amount: 60
                        currency: "USD"
                    }
                }
            }
        }
    }


                                    server
                                      |
                         -----------------------
                        |                       |
        -----------------------            ---------------     
       |           |           |          |       |       |
    servlet     servlet     servlet     table   table   table - [property]
                   | 
                   | 1
                   |
                leveldb

                        hash mod
    tableName, objectId --------> servlet

    GetObjectContext(tableName string, objectId string) (*Table, *Servlet)

event
table
db
property
objectId

/tables/{name}/objects/{objectId}/events/{timestamp}

event schema
------------

::

        key:    msgpack([]string{tableName, objectId}) 
        value:  [
            {timestamp:x, data:y} // state

            {timestamp:x, data:y} // events
            {timestamp:x, data:y} // events
            ...
        ]

data type
---------

property
########

- transient

- permanent  

data
####

- string

- integer

- float

- boolean

- factor


terms
-----

- table

- servlet


server
------

::

    server := skyd.NewServer(port, dataDir)
    server.ListenAndServe(c)


handlers
--------

- table handler

  ::

        curl -X GET http://localhost:8585/tables
        curl -X GET http://localhost:8585/tables/users
        curl -X POST http://localhost:8585/tables -d '{"name":"users"}'
        curl -X DELETE http://localhost:8585/tables/users

- property handler

  ::

        curl http://localhost:8585/tables/users/properties
        curl -X POST http://localhost:8585/tables/users/properties -d '{"name":"username","transient":false,"dataType":"string"}'
        curl http://localhost:8585/tables/users/properties/username
        curl -X PATCH http://localhost:8585/tables/users/properties/username -d '{"name":"username2"}'
        curl -X DELETE http://localhost:8585/tables/users/properties/username2

- event handler

  ::

        curl http://localhost:8585/tables/users/objects/john/events
        curl -X DELETE http://localhost:8585/tables/users/objects/john/events
        curl http://localhost:8585/tables/users/objects/john/events/2012-01-20T00:00:00Z
        curl -X PUT http://localhost:8585/tables/users/objects/john/events/2012-01-20T00:00:00Z -d '{"data":{"username":"johnny1000"}}'


- query handler

  ::

        curl -X POST http://localhost:8585/tables/users/query -d '{
            "steps": [
                {"type":"selection","fields":[{"name":"count","expression":"count()"}]}
            ]
        }'

- stats handler

  ::

        curl -X GET http://localhost:8585/tables/users/stats

- misc

  ::

        curl http://localhost:8585/ping
