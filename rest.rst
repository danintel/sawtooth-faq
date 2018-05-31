Preliminary FAQ: Sawtooth REST API
==================
[`PREVIOUS`_ | `HOME`_ | `NEXT`_]

.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

What's the difference between the ``sawtooth-rest-api --bind`` and ``--connect`` options?
-------------------

``sawtooth-rest-api --bind`` (``-B``)
    specifies where your rest-api would listen. The default is http://localhost:8008
``sawtooth-rest-api --connect`` (``-C``)
    specifies where your rest-api can reach to the validator. The default is http://localhost:4004

Is the REST API at TCP port 8080 or 8008?
-------------------
TCP Port 8008. It was 8080 before the 1.0 release and old examples and diagrams may use the old port number.

What REST API commands are available?
-------------------

Use localhost to access the REST API from the Validator Docker container or from where the Validator is running.
For example, to get state history (equivalent to `sawtooth state list`) type:

::

    curl http://localhost:8008/state
From the Client Docker container, access from rest-api.  For example:

::

    curl http://rest-api:8008/state

These are the supported REST API commands:

POST /batches
    Submit a protobuf-formatted batch list to the validator
GET /batches
    Fetch a paginated list of batches from the validator
GET /batches/{batch_id}
    Fetch the specified batch
GET /batch_status
    Fetch the committed statuses for a set of batches
GET /state
    Fetch a paginated list of leaves for the current state, or relative to a particular head block
GET /state/{address}
    Fetch a particular leaf from the current state
GET /blocks
    Fetch a paginated list of blocks from the validator
GET /blocks/{block_id}
    Fetch the specified block
GET /transactions
    Fetch a paginated list of transactions from the validator.
GET /transactions/{transaction_id}
    Fetch the specified transaction
GET /peers
    Fetch a list of current peered validators

For more information, see the Sawtooth REST API Reference at
https://sawtooth.hyperledger.org/docs/core/releases/latest/rest_api.html

[`PREVIOUS`_ | `HOME`_ | `NEXT`_]

.. _PREVIOUS: client.rst
.. _HOME: README.md
.. _NEXT: docker.rst

© Copyright 2018, Intel Corporation.
