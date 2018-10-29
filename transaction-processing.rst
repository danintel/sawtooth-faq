Sawtooth FAQ: Transaction Processing
==================
[PREVIOUS_ | HOME_ | NEXT_]

.. contents::


Does a client send a transaction request to all the validators in the network?
-------------------
No, it would just need to send the batch to one validator, then that validator will broadcast the batch to the rest of its peers.
If the validator is down, your connection attempt from the app would fail.
The app could have error handling to try again (in a retry loop) or try another validator.

Does Sawtooth have a way to control what participants have access to what assets in the business network and under what conditions?
-------------------
Blockchains, including Sawtooth, can be deployed as permissioned networks, wherein transactions are visible to the participants of the permissioned network, but not visible to the general public.

What transaction processors are required?
-------------------
Just the Settings TP, ``settings`` .
The Validator Registry TP,  ``sawtooth_validator_registry`` , is required if you use PoET.

What does the Settings TP do?
-------------------
The Settings TP provides on-chain configs to be applied to the Sawtooth Validators, so that you can change operational parameters without restarting the validators or the whole sawtooth network.
Also, you could write your own settings-tp, that stores the settings the same way but enforces different rules on how they are updated.

Is there an example where the Settings TP is used in another TP?
------------------------------------
Yes. check out ``sawtooth.identity.allowed_keys`` in the Identity TP:
https://github.com/hyperledger/sawtooth-core/blob/master/families/identity/sawtooth_identity/processor/handler.py

Can different Validator Nodes have different Transaction Processors running?
-------------------
No. The set of TPs must be the same for all validator nodes in a Sawtooth network.
The TP versions must also match across nodes--support the same set of ops.
This is so the transaction and state validation will be successful.

How do I support multiple versions of a Transaction Processor?
---------------------
You have two choices:

* A single TP can register itself to handle multiple versions. When the TP receives a transaction, it looks at the transaction's version field and decides how to handle it in the Apply() method.
* Multiple TPs, each handling a specific version.

In any case, all nodes need to support the same set of versions for a specific Transaction Family.

How do I support multiple Transaction Families in a Transaction Processor?
--------------------------------
This is usually not a preferred best practice.
But if the functionality of the different TFs are closely related, you can have a TP support multiple TFs.  Just have the TP register multiple TFs at startup, instead of just one TF.

How do I upgrade a transaction processor version?
---------------------------------
Bump up the version number of the TP and register with the validator. Submit transactions to the TP with the updated version number. If you want to reuse the existing TP, then you'll need to stop the existing one and register the new one.

Can a Validator Node have multiple TPs (processes) running for the same TF?
---------------------------------
Yes, one or more TPs, handling the same or different Transaction Families, may be running and register with a validator.
This is one way to achieve parallelism.
Another way to achieve parallelism is to write a multi-threaded TP.
The transactions are sent to transaction processors supporting the same transaction family in a round-robin fashion.

What are inputs and outputs in Sawtooth?
-------------------------------------
In a Sawtooth transaction, inputs list what are the inputs for the transaction (what addresses the TP can read). Outputs list what are the outputs for the transaction (what addresses the TP can modify). The inputs and outputs lists are specific to a transaction. See https://sawtooth.hyperledger.org/docs/core/releases/1.0/architecture/transactions_and_batches.html

Why use round-robin if the transaction processors are identical?
--------------------------------------------------------
This is useful when the when the validator's parallel scheduler is used.
Multiple transactions can be processed in parallel when the inputs/outputs do not conflict.

Where do I deploy transaction processors?
------------------------------------
Each validator node runs all transaction processors supported for the Sawtooth network.
Sawtooth includes features for asynchronously deploying and upgrading the Transaction Processors.
In a typical deployment you will have multiple Transaction Processors.

What happens if a validator receives a transaction but does not have a TP for it?
---------------------------------------------
If a validator receives a transaction that it does not have a transaction processor for, the validator will wait until a TP connects that can handle that transaction.
The validator will stay online and participate with the network and other services, but it will not be able to validate transactions for which it does not have the associated Transaction Processor.
 That validator would fall behind the rest on the network while it waits.
Hence it will not update state for any state transitions that include or depend on such transactions *until* the transaction processor is deployed for that node.
Once deployed on that validator, the validator will be able to catch up with the network.

How can I limit what Transaction Processors run on a Validator Node?
-------------------
You can also limit which transactions are accepted on the network by setting
``sawtooth.validator.transaction_families`` If that setting is not set, all transaction processors are accepted.
This setting is ignored in dev-mode consensus.

Where do transactions originate?
--------------------------------
From the client. The client sends a transaction to a validator, in a batch with one or more transactions. The transactions are sent to the validator, via the REST API, for the validator to add to the blockchain.

Can the same transaction appear in multiple blocks?
--------------------------------
No. Each block has a unique set of transaction. A block is composed of batches, which is composed of transactions. Each transaction has a unique ID and appears only once in a blockchain. There may be, however, differences in ordering of blocks at a validator due to scheduling, transaction dependencies, etc.


What mechanism prevents a rogue TP from operating and corrupting data?
------------------------------
The design is as such that rogue TPs can't harm legitimate TPs. When you run a network of validators, each validator has to have same version of TPs. If a rogue TP is modifying your TPs data, the same TP has to run in the rest of the validators in the network, to be able to affect the blockchain. The validator where the rogue TP is working will constantly fail state validations(Merkle hashes will be different with rest of the network). Hence, the bigger the validator network, the more robust it is against such attacks.

What does this error mean: ``processor | [... DEBUG executor] transaction processors registered for processor type cryptomoji: 0.1`?
-----------------------
It means there is no transaction processor running for your transaction family.


What does this error mean: ``processor | { AuthorizationException: Tried to get unauthorized address ...`` ?
-----------------------
It means a the transaction processor tried to access (get/put) a value not in the list of inputs/outputs. This occurs when a client submits a transaction with an inaccurate list of inputs/outputs.

Make sure the Sawtooth address is the correct length--the address is 70 hex characters, which represent a 35 byte address (including the 6 hex character or 3 byte Transaction Family prefix).

What does this error mean: ``applicator->Apply errorState Get Authorization error. Check transaction inputs.``
--------------------
See the answer above.

If you have a large file to store, is it best to just record the file hash and store the file offline?
---------------------------------------
It depends on your use case. Storing data off-chain has a big downside.
Although you can confirm it hasn't been tampered with with the on-chain hash, there is nothing stopping the file from disappearing.
Also, how do you make sure everyone who needs the data can get to it?

If I register a transaction processor to one validator, does the registration get transmitted to the other validators in a network?
----------------------------
No. Your transaction processor must be deployed to all validators.  All validators in a network must have the same set of transaction processors.

How do I add a transaction processor?
--------------------
You just start it in for all the validator nodes. The TP needs to connect to ``tcp://localhost:4004`` or, if you are using Docker, ``tcp://validator:4004``

How do I restrict what transaction processors are allowed?
--------------------
By default, any TP can be added to a node without special permission (other than network access). To restrict what TPs can be added to a validator, use ``sawset proposal create`` to set ``sawtooth.validator.transaction_families``.
For details, see ``Configuring the List of Transaction Families`` at https://sawtooth.hyperledger.org/docs/core/releases/latest/app_developers_guide/docker.html

How do I add events to the transaction processor?
--------------------
In the TP code, call ``context.add_event()``.
This adds a an application-specific event.
In the client code (or other app for listening), subscribe to the event.
For details, see
https://sawtooth.hyperledger.org/docs/core/releases/latest/architecture/events_and_transactions_receipts.html#events

What initial Sawtooth events are available?
-------------------
Besides application-specific events, the Sawtooth default events are:

``sawtooth/commit-block``
    Committed block information: block ID, number, sate root hash, and previous block ID
``sawtooth/state-delta``
    All state changes that occurred for a block at a specific address

How do I subscribe to Sawtooth events?
-----------------------------
See the documentation at
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/event_subscriptions.html
Here are examples in Python and Javascript:

* https://github.com/danintel/sawtooth-cookiejar/blob/master/events/events_client.py
* https://github.com/hyperledger/sawtooth-supply-chain/blob/master/ledger_sync/subscriber/index.js

How do I handle forks while subscribing to Sawtooth events?
-------------------------
If you get ``fork_detected: true`` in the ``state_changes`` object,
you delete or undo events labeled with the blocks that have been removed from history. For example, if you added the events to a local database, remove the rows labeled with the removed blocks.  Then apply events forward from the most recent common block.

Why is the Apply method in the TP handler called twice?
--------------------------------------------
That is by design. It can be called more than twice.
For that reason, the TP handler must be deterministic
(have the same output results given the same input).

What does it mean to be deterministic?
------------------------
Deterministic means the output never varies, given the same input.  That is,

* serialization must be deterministic, meaning the encoding is always in the same order and always the same for the same data
* timestamps cannot be generated by the TP as they chain (timestamps in a transaction from the client are OK as they don't change for a given transaction)
* counters, likewise, generated by the TP are not allowed (but counters from the client are OK for a given transaction)

Do Transaction Processors run off-chain or on-chain?
--------------------------------
Sawtooth TPs run off-chain, as a process (or processes).

My TP throws an exception of type ``InternalError``, but the ``Apply`` method gets stuck in an endless loop
---------------------------------
``InternalError`` is supposed to be a transient error (some internal fault like 'out of memory' that is temporary), and may succeed if retried.
The validator retries the transaction with the TP and results in a loop.
 If the transaction is invalid, you probably want to raise an ``InvalidTransaction`` error instead.
Bottom line&mdash;internal errors are retried, and invalid transactions are not retried.

I get this error when I try to set some Sawtooth settings: ``Chain head is not set yet. Permit all``
--------------------
This error has been seen when the directory or file ownerships are wrong. Try setting ownership as follows: ``chown sawtooth:sawtooth /var/lib/sawtooth /var/lib/sawtooth/*`` .
Then verify with ``ls -la /var/lib/sawtooth`` .
Another cause could be because there is no genesis block.

Does the Transaction Processor know the current Transaction ID?
---------------------------------
Yes. It is available in the header.
The transaction header_signature is the Transaction ID.

Can I run two different Transaction Processors on the same Sawtooth Network?
---------------------
Yes, you can run any number of transaction families, for example, you can r un the Seafood Supply Chain app and Bond Asset Settlement app on the same network.

What happens if someone writes a fake Transaction Processor (with the same name, version, and address space) that can access and modify state data?
---------------------------------
The fake TP will cause the node to fork and it will be ignored by the rest of the network.

Why is there no timestamp in a transaction header or block?
--------------------------------------------------
Using timestamps in a distributed network is troublesome--mostly due to complex clock synchronization issues among peers. You could add a timestamp in your transaction family's transaction payload.

Sawtooth stores a timestamp in the block if the network is setup to inject BlockInfo transactions using the BlockInfo Transaction Family (which is used for EVM compatibility). See: https://sawtooth.hyperledger.org/docs/core/releases/latest/transaction_family_specifications/blockinfo_transaction_family.html


Does Sawtooth allow multiple digital signatures on a single transaction?
-------------------------
In Sawtooth the "batch" is the atomic unit of change. This is a collection of one or more individually signed transactions. You could have multiple transactions, each signed by a different party, combined in one batch. This would have a similar effect to what you are talking about I think.
You can also build whatever app logic you like. So you can require transactions from multiple parties before an action is taken.
The individual transactions themselves have only one signer.

What is the size limit for a Sawtooth transaction?
----------------------------------------
There is no size limit, barring any memory and storage limits for your Sawtooth nodes.

If you don't want to write a large transaction, you can reference some external source (and also save a checksum). The disadvantage of storing data externally is it's not replicated across nodes and may be lost.

What does this message mean: ``Did not respond to the ping, removing transaction processor`` ?
-----------------------------------
This is a message from the Hyperledger Sawtooth blockchain's Validator. A timeout occurred when the Validator was checking connections with all the registered transaction processors. If a transaction processor does not respond, it is removed from the list.

Some possible causes: the transaction processor (TP) died. Check that the TP process is still running (check in the Docker container if you are running docker). Check network connectivity if the TP is on another host or another virtual machine. Check the message logs. Perhaps the TP is "frozen" or hanging or has a bug. Add logging messages (using LOGGER).

What does this message mean: ``Block . . . rejected due to state root hash mismatch`` ?
----------------------------
You have a transaction processor that implements some non-deterministic behavior, such as generating a random number in a calculation, or a timestamp, etc.

What does this message mean: ``Have not received a chain head from peers.`` ?
-----------------------------
This message has been seen when a node is not running a needed transaction processor.  A new node needs to run all the transaction processors required for all the supported transaction families in this Sawtooth blockchain network.

How do I debug a transaction processor?
---------------------------
One way is to add logging messages (using LOGGER) and sprinkle your code with debug messages, such as ``LOGGER.info("Action = %s.", action)`` in Python (or another language you use for the TP). Start the transaction processor with the ``-vv`` or ``-vvv`` flags and look for console output.

What does this message mean: ``failing transaction ... since it isn't required in the configuration`` ?
----------------------------
It means you set the ``sawtooth.validator.transaction_families`` setting with the Settings TP and did not include the TP name and version for the transaction that failed.  The fix is to add the TP name and version to the setting.


[PREVIOUS_ | HOME_ | NEXT_]

.. _PREVIOUS: installation.rst
.. _HOME: README.rst
.. _NEXT: validator.rst

© Copyright 2018, Intel Corporation.
