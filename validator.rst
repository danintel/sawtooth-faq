Unofficial FAQ: Sawtooth Validator
==================
[PREVIOUS_ | HOME_ | NEXT_]

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

Here is a gist with brief instructions for a 2-node network:
https://gist.github.com/askmish/a23bde6f2e59e4256be8afe965a9166b

The important part about configuring a multi-node network is
to create a genesis block only with the first validator.  Do not create multiple genesis blocks with subsequent validators (that is do not run ``sawset genesis`` and ``sawadm genesis``).

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
accepted on the network with the ``sawtooth.validator.transaction_families``
setting.  If that setting is not set, all transaction would be accepted.

I set sawtooth.validator.transaction_families as follows (from the documentation) but it's ignored
-------------------
The sawtooth.validator.transaction_families setting is ignored using dev-mode consensus and does not need to be set.

What is the difference between ``sawtooth-validator --peers {list}`` and ``sawtooth-validator --seeds {list}``?
-------------------
There are two peering modes in sawtooth: static and dynamic. The static peering mode requires the ``--peers`` arg to connect to other peer validators. Whereas, in the dynamic peering mode the ``--peers`` if specified will be processed and then use ``--seeds`` for the initial connection to the validator network and to start topology build-out (discovery and connection to more peer validators).

What files does Sawtooth use?
-------------------
``/var/lib/sawtooth/``
    contains the blockchain, Merkle tree, and transaction receipts
``/var/log/sawtooth/``
    contains log files
``~/.sawtooth/keys/``
    contain one or more sets of user key pairs
``/etc/sawtooth/keys/``
    contain the validator key pair
``/etc/sawtooth/policy/``
    contains policy settings, if any

Why does the validator create large 1TByte files?
-------------------
The large 1TByte files in ``/var/lib/sawtooth/`` are "sparse" files, implemented with LMDB (Lightning Memory-mapped Database).  They are random-access files with mostly empty blocks. They do not actually consume 1Tbyte of storage.

What TCP ports does Sawtooth use?
-------------------
* 4004 is used by the Validator component bus, which uses ZMQ. The validator listens to requests on this port from the REST API and from one or more transaction processors.
This port can be left closed to external hosts in a firewall configuration if all the transaction processors are on the same host as the validator (the usual case)

Port 4004 is sometimes exported to port 4040 in Sawtooth Docker containers for the validator.

* 8008 is used by the REST API, which connects the Client to the Validator.
This port can be left closed to external hosts in a firewall configuration if the client is always on the same host as a validator (common during testing)

* 8800 is used by the Validator network to communicate with other Validators.
This port needs to be left open to external hosts in a firewall configuration to communicate with peer validators

How do I create a Sawtooth Network?
-------------------
See *Creating a Sawtooth Network* at
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/creating_sawtooth_network.html

Create the genesis block only one time, on the first node, and configure one or more peer Validator nodes for each node.

Can I run two validators on the same machine?
-------------------
Yes, but it is not recommended.  You need to configure separate Sawtooth instances with different:

* data, key, log, and policy directories (default values listed above).
If ``$SAWTOOTH_HOME`` is set, all these directories are under ``$SAWTOOTH_HOME``.
It's not recommended, but you can also can also change the directories in ``path.toml``.
For more information, see
https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/configuring_sawtooth/path_configuration_file.html

* REST API TCP port (default 8008).  Change in ``rest-api.toml``. For details, see
https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/configuring_sawtooth/rest_api_configuration_file.html

* Validator TCP ports (default of 8800 for the peer network and 4004 for the validator components).  Change with the ``bind`` setting in ``validator.toml``.
For details, see
https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/configuring_sawtooth/validator_configuration_file.html

* Genesis block. This is important. As with validators on multiple machines (the usual case), it's important to create a genesis block only with the first validator.  Do not create multiple genesis blocks with subsequent validators (that is do not run ``sawset genesis`` and ``sawadm genesis``)

Instead, consider setting up separate virtual machines (such as with VirtualBox) for each validator.  This ensures isolation of files and ports for each Validator.

What is the validator parallel scheduler?
---------------------------------------
The validator has two schedulers--parallel and serial.
The parallel scheduler gives a performance boost because it allows multiple transactions to be processed at the same time when the transaction inputs/outputs do not conflict.
The scheduler is specified with the
``sawtooth-validator --scheduler {parallel,serial}`` option.
The current default is ``serial``, but it may change to ``parallel`` in the future.

What does this error mean: ``[... DEBUG client_handlers] Unable to find entry at address ...``?
-----------------------
It means the address doesn't exist.
I've seen this error when retrieving a value that should have been written, but was not written.
The reason was because the transaction processor for the value was not running so the object at the address was never created.

What does this error mean: ``sawtooth-validator[... ERROR cli] Cannot have a genesis_batch_file and an existing chain``?
-----------------------
You tried to create a new genesis block when you did not need to (because there already is a genesis block).

I get this error when testing with a lot of validators: ``Max occupancy was not provided by transaction processor: ... Using default max occupancy: 10``
----------------------------------
You need to set the number of validators if it's over 10.
For example, in ``/etc/sawtooth/validator.toml`` set ``maximum_peer_connectivity = 50``
See https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/configuring_sawtooth/validator_configuration_file.html


[PREVIOUS_ | HOME_ | NEXT_]

.. _PREVIOUS: transaction-processing.rst
.. _HOME: README.rst
.. _NEXT: consensus.rst

© Copyright 2018, Intel Corporation.
