Unofficial FAQ: Sawtooth Validator
==================
[`PREVIOUS`_ | `HOME`_ | `NEXT`_]

.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

What validation does the validator do?
-----------------------
At a high-level, the Validator verifies the following:

* Permission - Check the batch signing key against the allowed transactor permissions

* Signature - Check for integrity of the data

* Structure - Check structural composition of batches: duplicate transactions, extra transactions, etc.

Is there a simple example to show how to run Sawtooth
-------------------
See these instructions to install and use Sawtooth with Docker, Ubuntu Linux, or AWS:
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/installing_sawtooth.html

Is there an example for a multiple node Sawtooth Network?
-------------------
See these instructions for setting up a 5-node Sawtooth Network with PoET Simulator Consensus using Docker:
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/creating_sawtooth_network.htmlow do I add a node to a Sawtooth Network?

How do I add a node to a Sawtooth Network?
-------------------

See
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/creating_sawtooth_network.html#ubuntu-add-a-node-to-the-single-node-environment

How do I verify the Validator is running and reachable?
-------------------
Run the following command from the Validator Docker container or from where the Validator is running:

::

        curl http://localhost:8008/blocks

This verifies the REST API is available.

From the Client Docker container run this:

::

        curl http://rest-api:8008/blocks

You should see a JSON response similar to this:

::

    {
      "data": [
        {
          "batches": [
            {
              "header": {
                "signer_public_key": . . .

Do all validators need to run the same transaction processors?
-------------------
Yes.  All validators must run all of the same transaction processors that are
on the network. If a validator receives a transaction that it does not have a
transaction processor for, the validator will wait until a transaction processor
connects that can handle that transaction. That validator would fall behind the
rest on the network while it waits. You can also limit which transactions are
accepted on the network with the `sawtooth.validator.transaction_families`
setting.  If that setting is not set, all transaction would be accepted.

I set sawtooth.validator.transaction_families as follows (from the documentation) but it's ignored
-------------------
The sawtooth.validator.transaction_families setting is ignored using dev-mode consensus and does not need to be set.

What is the difference between `sawtooth-validator --peers {list}` and `sawtooth-validator --seeds {list}`?
-------------------
There are two peering modes in sawtooth: static and dynamic. The static peering mode requires the `--peers` arg to connect to other peer validators. Whereas, in the dynamic peering mode the `--peers` if specified will be processed and then use `--seeds` for the initial connection to the validator network and to start topology build-out (discovery and connection to more peer validators).

What files does Sawtooth use?
-------------------
``/var/lib/sawtooth/``
    contains the blockchain, Merkle tree, and transaction receipts
``~/.sawtooth/keys/``
    contain one or more sets of user key pairs
``/etc/sawtooth/keys/``
    contain the validator key pair

Why does the validator create large 1TByte files?
-------------------
The large 1TByte files in ``/var/lib/sawtooth/`` are "sparse" files, implemented with LMDB (Lightning Memory-mapped Database).  They are random-access files with mostly empty blocks. They do not actually consume 1Tbyte of storage.

What TCP ports does Sawtooth use?
-------------------
* 4004 is used by the Validator component bus, which uses ZMQ. The validator listens to requests on this port from the REST API and from one or more transaction processors

* 8008 is used by the REST API, which contects the Client to the Validator

* 8800 is used by the Validator network to communicate with other Validators

How do I create a Sawtooth Network?
-------------------
See *Creating a Sawtooth Network* at
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/creating_sawtooth_network.html

Create the genesis block only one time, on the first node, and configure one or more peer Validator nodes for each node.

Can I run two validators on the same machine?
-------------------
Yes, but it is not recommended.  You need to configure separate Sawtooth instances with different:

* data and key directories (listed above)

* TCP ports (8008, 4004, and 8800, listed above)

Instead, consider setting up separate virtual machines (such as with VirtualBox) for each validator.  This ensures isolation of files and ports for each Validator.

[`PREVIOUS`_ | `HOME`_ | `NEXT`_]

.. _PREVIOUS: transaction-processing.rst
.. _HOME: README.rst
.. _NEXT: consensus.rst

© Copyright 2018, Intel Corporation.
