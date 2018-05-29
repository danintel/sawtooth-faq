FAQ: Sawtooth Transaction Processing
==================
[`PREVIOUS`_] [`NEXT`_]
-----------------------

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
Just the Settings TP.  All the other TPs are optional.

What does the Settings TP do?
-------------------
The settings-tp provides on-chain configs to be applied to the Sawtooth Validators, so that you can change operational parameters without restarting the validators or the whole sawtooth network.
Also, you could right your own settings-tp, that stores the settings the same way but enforces different rules on how they are updated.

Can different Validator Nodes have different Transaction Processors running?
-------------------
No. The set of TPs must be the same for all validator nodes in a Sawtooth network.
The TP versions must also match--support the same set of ops.
This is so the transaction and state validation will be successful.

What happens if a validator receives a transaction but does not have a TP for it?
---------------------------------------------
If a validator receives a transaction that it does not have a transaction processor for, the validator will wait until a TP connects that can handle that transaction. That validator would fall behind the rest on the network while it waits.

How can I limit what Transaction Processors run on a Validator Node?
-------------------
You can also limit which transactions are accepted on the network by setting
`sawtooth.validator.transaction_families` If that setting is not set, all transaction processors are accepted.
This setting is ignored in dev-mode consensus.

What mechanism prevents a rogue TP from operating and corrupting data?
------------------------------
The design is as such that rogue TPs can't harm legitimate TPs. When you run a network of validators, each validator has to have same version of TPs. If a rogue TP is modifying your TPs data, the same TP has to run in the rest of the validators in the network, to be able to affect the blockchain.  The validator where the rogue TP is working will constantly fail state validations(merkle hashes will be different with rest of the network).  Hence, the bigger the validator network, the more robust it is against such attacks.

[`PREVIOUS`_] [`NEXT`_]
=========

.. _PREVIOUS: sawtooth.rst
.. _NEXT: validator.rst

© Copyright 2018, Intel Corporation.
