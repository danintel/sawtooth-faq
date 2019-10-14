Sawtooth FAQ: Validator
=======================

[PREVIOUS_ | FAQ_ | NEXT_]

.. contents::


What validation does the validator do?
--------------------------------------
At a high-level, the Validator verifies the following:

* Permission - Check the batch signing key against the allowed transactor permissions

* Signature - Check for integrity of the data

* Structure - Check structural composition of batches: duplicate transactions, extra transactions, etc.

Is there a simple example to show how to run Sawtooth
-----------------------------------------------------
See these instructions to install and use Sawtooth with Docker, Ubuntu Linux, or AWS:
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/installing_sawtooth.html

Is there an example for a multiple node Sawtooth Network?
---------------------------------------------------------
See these instructions for setting up a 5-node Sawtooth Network with PoET CFT Consensus using Docker:
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/creating_sawtooth_network.html

Here is a gist with brief instructions for a 2-node network:
https://gist.github.com/askmish/a23bde6f2e59e4256be8afe965a9166b

The important part about configuring a multi-node network is
to create a genesis block only with the first validator. Do not create multiple genesis blocks with subsequent validators (that is do not re-run ``sawset genesis`` and ``sawadm genesis``).

How do I add a node to a Sawtooth Network?
------------------------------------------
See
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/creating_sawtooth_network.html#ubuntu-add-a-node-to-the-single-node-environment

How do I verify the Validator is running and reachable?
-------------------------------------------------------
Run the following command from the Validator Docker container or from where the Validator is running:

What do I do if some of the Sawtooth Network nodes go offline?
--------------------------------------------------------------
You can restart any failed nodes. They should rejoin the network and will then process all blocks that were added to the blockchain since the node went down. It will be busy during this initial phase, but will return to normal after that.

.. code:: sh

        curl http://localhost:8008/blocks

This verifies the REST API is available.

From the Client Docker container run this:

.. code:: sh

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
--------------------------------------------------------------
Yes. All validators must run all of the same transaction processors that are
on the network. If a validator receives a transaction that it does not have a
transaction processor for, the validator will wait until a transaction processor
connects that can handle that transaction. That validator would fall behind the
rest on the network while it waits. You can also limit which transactions are
accepted on the network with the ``sawtooth.validator.transaction_families``
setting. If that setting is not set, all transaction would be accepted.

I set sawtooth.validator.transaction_families as follows (from the documentation) but it's ignored
--------------------------------------------------------------------------------------------------
The sawtooth.validator.transaction_families setting is ignored using dev-mode consensus and does not need to be set.

What is the difference between ``sawtooth-validator --peers {list}`` and ``sawtooth-validator --seeds {list}``?
---------------------------------------------------------------------------------------------------------------
There are two peering modes in sawtooth: static and dynamic  (use option ``--peering static`` or ``--peering dynamic`` depending on which mode is required). The static peering mode requires the ``--peers`` arg to connect to other peer validators. Whereas, in the dynamic peering mode the ``--peers`` if specified will be processed and then use ``--seeds`` for the initial connection to the validator network and to start topology build-out (discovery and connection to more peer validators).

For static peering do I need to specify all validator nodes, or just some of them?
----------------------------------------------------------------------------------
For static, you need to specify all nodes. I recommend dynamic peering where you don't need to specify all of them, just a good sampling (with --seeds). The rest will be discovered. All dynamic peers have to specified by at least one other node (and preferably multiple nodes).

What files does Sawtooth use?
-----------------------------
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
``/var/lib/sawtooth-raft/``
    Optional Raft consensus-specific files.  Present only if you use Raft.  Directory can be changed with ``$SAWTOOTH_RAFT_HOME``

If set these files are placed under directory ``$SAWTOOTH_HOME`` (except files under your home directory, ``~`` ). Detailed configuration information and examples for Sawtooth directories is at ``/etc/sawtooth/path.toml.example``

Why does the validator create large 1TByte files?
-------------------------------------------------
The large 1TByte files in ``/var/lib/sawtooth/`` are "sparse" files, implemented with LMDB (Lightning Memory-mapped Database). They are random-access files with mostly empty blocks. They do not actually consume 1Tbyte of storage.

How do I backup a large sparse file?
------------------------------------
One method to backup it up is to use the ``tar -S`` option (sparse option). For example: ``tar cSf merkle-00.tar merkle-00.*`` . Some of the Linux file tools have similar options, such as ``cp --sparse``.

For LMDB databases, the database should be backed up when it is quiet (no updates). If the database is "live", it's best to do a backup by dumping it to a file. That will avoid inconsistencies from backups during the middle of updates. Use ``mdb_dump`` from package ``lmdb-utils`` . For example,
``mdb_dump -n /var/lib/sawtooth/block-00.lmdb >block-00.lmdb.dump``
Use ``mdb_load -n -f block-00.lmdb.dump`` to restore the database.

What does ``lmdb.CorruptedError: mdb_put: MDB_CORRUPTED: Located page was wrong type`` mean?
--------------------------------------------------------------------------------------------
The LMDB database, which stores the blockchain, is corrupted.
The blockchain is backed-up automatically with multiple nodes.
There are no published recovery tools, but you could clean out the data on the failed machine and restart and then allow the chain to be rebuilt from its peers.

What TCP ports does Sawtooth use?
---------------------------------
* 4004 is used by the Validator component bus, which uses ZMQ. The validator listens to requests on this port from the REST API and from one or more transaction processors.
  This port can be closed to external hosts in a firewall configuration if all the transaction processors are on the same host as the validator (the usual case)

  Port 4004 is sometimes exported to port 4040 in Sawtooth Docker containers for the validator.

* 8008 is used by the REST API, which connects the Client to the Validator.
  This port should be closed to external hosts in a firewall configuration if the client is always on the same host as a validator. This port is not encrypted.
  If the client connecting to the REST API is external to the host, I recommend placing a TLS/HTTPS proxy in front this port and adding authentication.

* 8800 is used by the Validator network to communicate with other Validators.
  This port needs to be open to external hosts in a firewall configuration to communicate with peer validators. Uses ZMQ.

* 5050 is used by the consensus engine (such as PoET or Raft).
  This port should be closed to external hosts in a firewall configuration. Uses ZMQ.

* 3030 is used by the Seth TP (if you have Seth running).
  It is used to send JSON-RPC EVM requests and is not encrypted.
  This port should be blocked from external access because one can do admin operations from it.

Sawtooth does not use UDP ports (only TCP).

How do I create a Sawtooth Network?
-----------------------------------
See *Creating a Sawtooth Network* at
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/creating_sawtooth_network.html

Create the genesis block only one time, on the first node, and configure one or more peer Validator nodes for each node.

I have Sawtooth running with a single node. How do I add a node?
----------------------------------------------------------------
You need to either start up the validator with information about the network peers using the ``sawtooth-validator --peers`` option or set ``seeds`` or ``peers`` in configuration file ``/etc/sawtooth/validator.toml``. Then restart the node.

Can I run two validators on the same machine?
---------------------------------------------
Yes, but it is not recommended. You need to configure separate Sawtooth instances with different:

* data, key, log, and policy directories (default values listed above).
  If ``$SAWTOOTH_HOME`` is set, all these directories are under ``$SAWTOOTH_HOME``.
  It's not recommended, but you can also can also change the directories in ``path.toml``.
  For more information, see
  https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/configuring_sawtooth/path_configuration_file.html

* REST API TCP port (default 8008). Change in ``rest-api.toml``. For details, see
  https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/configuring_sawtooth/rest_api_configuration_file.html

* Validator TCP ports (default of 8800 for the peer network and 4004 for the validator components). Change with the ``bind`` setting in ``validator.toml``.
  For details, see
  https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/configuring_sawtooth/  validator_configuration_file.html

* Genesis block. This is important. As with validators on multiple machines (the usual case), it's important to create a genesis block only with the first validator. Do not create multiple genesis blocks with subsequent validators (that is do not run ``sawset genesis`` and ``sawadm genesis``)

  Instead, consider setting up separate virtual machines (such as with VirtualBox) for each validator. This ensures isolation of files and ports for each Validator.

What TCP ports should I restrict or allow through a firewall?
-------------------------------------------------------------
* TCP Port 4004 is used for internal validator / transaction processor communications. Restrict from outside use
* TCP Port 8008 is used by the REST API for validator / client communications. Restrict from outside use if the client resides on the host
* TCP Port 8080 is used to communicate between validator nodes. Allow

What is the validator parallel scheduler?
-----------------------------------------
The validator has two schedulers--parallel and serial.
The parallel scheduler gives a performance boost because it allows multiple transactions to be processed at the same time when the transaction inputs/outputs do not conflict.
The scheduler is specified with the
``sawtooth-validator --scheduler {parallel,serial}`` option.
The current default is ``serial`` for Sawtooth 1.1 and earlier and ``parallel`` for the (unreleased) Sawtooth nightly build.
For example:
``sawtooth-validator --scheduler parallel -vv`` .

What are the verbosity levels of the various Sawtooth CLIs?
-----------------------------------------------------------
* ``-v`` means warning messages
* ``-vv`` means information + warning messages
* ``-vvv`` means debug + information + warning messages

After a failed transaction, the validator stops processing further transactions. What can I do?
-----------------------------------------------------------------------------------------------
You can run the validator in parallel processing mode.
For a serial scheduler, a failed transaction will be retried and no further transactions can be processed until the blocked transaction is processed successfully. Parallel scheduling will cause non-dependent transactions to be scheduled irrespective of the failed transaction.

How can I improve Sawtooth performance?
---------------------------------------
* First, for performance measurement or tuning, do not run the default "dev mode" consensus algorithm. Run another one, such as PoET SGX or PoET CFT. Dev mode is not for production use and excessive forks under heavy use degrades performance
* Run the validator in parallel mode, not serial mode
* Consider increasing the on-chain setting ``sawtooth.publisher.max_batches_per_block`` . Try a value of 200 batches per block to start with. This and other on-chain settings can be changed on-the-fly without impacting older blocks.
* Run multiple transaction processors per validator node for the same transaction family. This is especially useful for TPs written in Python
* Batch multiple transactions together as much as possible in a Batch of transaction or a BatchList of multiple transactions (or both)
* Write the transaction processor in a thread-friendly programming language such as Rust or C++, not Python. Python is an interpretive language and therefore slower. It also suffers from the Global Interpreter Lock (GIL), which locks executing multiple threads to one thread at-a-time
* When fully stabilized, substitute PoET consensus with Raft consensus. Raft is CFT instead of BFT, but it should perform better in exchange for lower fault tolerance
* As you make changes, measure the impact with a performance tool such as Hyperledger Caliper

Is there any way to get real-time Sawtooth statistics?
------------------------------------------------------
Yes. Sawtooth has Telegraf/InfluxDB/Grafana to gather and display metrics.
Install the packages and follow these instructions:
https://sawtooth.hyperledger.org/docs/core/nightly/master/sysadmin_guide/grafana_configuration.html

Here is a Sawtooth Grafana screenshot: https://twitter.com/liedenavilla/status/1042792583221653504

What does this error mean: ``[... DEBUG client_handlers] Unable to find entry at address ...``?
-----------------------------------------------------------------------------------------------
It means the address doesn't exist.
I've seen this error when retrieving a value that should have been written, but was not written.
The reason was because the transaction processor for the value was not running so the object at the address was never created.

What does this error mean: ``sawtooth-validator[... ERROR cli] Cannot have a genesis_batch_file and an existing chain``?
------------------------------------------------------------------------------------------------------------------------
You tried to create a new genesis block when you did not need to (because there already is a genesis block). To solve, this remove file ``/var/lib/sawtooth/genesis.batch.file`` and restart ``sawtooth-validator`` .

I get this warning when running a transaction processor: ``Max occupancy was not provided by transaction processor: ... Using default max occupancy: 10``
---------------------------------------------------------------------------------------------------------------------------------------------------------
Max occupancy is the number of transactions that a transaction processor can handle. By default it is set to 10, in this case if more than 10 transactions are supplied at once validator will wait until occupancy is available for the processor or is cancelled. This is introduced so that transaction processor is not overwhelmed with requests.

You need to set the number of validators if it's over 10.
For example, in ``/etc/sawtooth/validator.toml`` set ``maximum_peer_connectivity = 50``
See https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/configuring_sawtooth/validator_configuration_file.html
You can also use the `sawtooth-validator --maximum-peer-connectivity`
command line option.

I start the validator, but it's stuck at this message: ``Waiting for transaction processor (sawtooth_settings, 1.0)``
---------------------------------------------------------------------------------------------------------------------
The Sawtooth Settings TP is mandatory for all Sawtooth nodes--even if you don't add or change any settings. You probably want to also start the TP for your desired application. To start the Settings TP, type: ``sudo -u sawtooth settings-tp -v``

Can I change Sawtooth settings after genesis?
---------------------------------------------
Yes, but you are limited to using the rule that is currently set for changing settings. This is handled by the Settings TP.

Why am I getting this validator message: ``Reject building on block 8c5ebbea: Validator is claiming blocks too frequently.``
----------------------------------------------------------------------------------------------------------------------------
It is from the z-test, which is a defense-in-depth mechanism to catch validators that are publishing blocks with an improbable frequency. Unfortunately the defaults we chose for that statistical test aren't well suited for tiny networks (that feature is really intended for added security in large production networks).
If you have only one validator, you are bound to fail the z-test eventually.
Probably the best way to fix that in your test network is to restart it with some different z-test settings. This will effectively disable z-test:
``sawtooth.poet.ztest_minimum_win_count = 999999999``


Why do I get a ``Block validation failed`` message from the validator?
----------------------------------------------------------------------
Usually block validation fails because of something non-deterministic in the transaction processor. This is usually because of the serialization method, which is usually because someone used JSON (use something like Protobufs or CBOR instead). Other common sources of non-determinism are relying on system time in the transaction processor logic.

What does this error mean: ``Network communications between validators will not be authenticated or encrypted.``
----------------------------------------------------------------------------------------------------------------
It means you did not configure your ``network_public_key`` and ``network_private_key`` in ``validator.toml``.

How do I generate the ``network_public_key`` and ``network_private_key`` in ``validator.toml`` ?
------------------------------------------------------------------------------------------------
These are the ZMQ message keys used to securely communicate with other nodes.

If you've installed sawtooth already, python3 and python3-zmq would have been already installed and available in your system.
Here's an example to create the keypair in Python:

.. code:: python

    import zmq
    (public, secret) = zmq.curve_keypair()
    print("network_public_key =", public.decode("utf-8"),
          "\nnetwork_private_key =", secret.decode("utf-8"))

Also, if you can use a compiled binary tool:

.. code:: sh

   $ sudo apt-get install g++ libzmq3-dev
   $ wget https://raw.githubusercontent.com/zeromq/libzmq/master/tools/curve_keygen.cpp
   $ g++ curve_keygen.cpp -o curve_keygen -lzmq
   $ ./curve_keygen

Copy the corresponding public key output to ``network_public_key`` and the private key output to ``network_private_key`` fields in ``validator.toml``

What does this warning mean: ``Network key pair is not configured, Network communications between validators will not be authenticated or encrypted`` ?
--------------------------------------------------------------------------------------------------------------------------------------------------------
You did not configure the keypair for the network nodes.  For development purposes, that is OK.  For production, use create a network keypair and add to file `validator.toml`, as instructed in the question here about how to generate the ``network_public_key`` and ``network_private_key`` .


I am seeing only one transaction per block in my blockchain. Why?
-----------------------------------------------------------------
The Sawtooth Validator combines transaction batches when possible. If you are using dev mode consensus, it is producing blocks as fast as possible, which will typically only contain one transaction. You can simulate what would happen on a real network by setting min and max block times for devmode. If you set min to 10 and max to 20, it will include many more transactions per block. You can also combine transactions from your client by submitting multiple transactions in a batch.

What does ``Block publishing suspended until new chain head arrives`` mean?
---------------------------------------------------------------------------
It means that a new block arrived and the receiving validator wants to stop creating the block it was working on until it finds the new chain head.

After adding 100,000 blockchain state variables, I run out of memory. Why?
--------------------------------------------------------------------------
Sawtooth stores the blockchain in a LMDB database at ``/var/lib/Sawtooth/block-00.lmdb`` . The LMDB database is a "sparse" file meaning no storage is allocated for the file until it is used (written to). The database should not run out of memory, as long as filesystem storage is available. The memory error could happen in Kubernetes or Docker or other virtual machine environments where there are no storage volumes mapped to the VM.

What are the maximum number of blocks in a Sawtooth blockchain?
---------------------------------------------------------------
There is no limit, other than the available storage for a node.

[PREVIOUS_ | FAQ_ | NEXT_]

.. _PREVIOUS: transaction-processing.rst
.. _FAQ: README.rst
.. _NEXT: consensus.rst

© Copyright 2018, Intel Corporation.
