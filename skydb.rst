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

::

    {
        user: {
            gender: F
            birth: 1986-10
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

- event handler

- query handler


