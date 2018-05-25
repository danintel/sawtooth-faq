FAQ: Sawtooth in General
========================

What is the difference between Hyperledger and Sawtooth?
--------------------------------------------------------

* Sawtooth (or Hyperledger Sawtooth) is blockchain software sponsored by Intel Corporation, but does not require Intel hardware.
* Hyperledger is a consortium of Linux blockchain software, including Sawtooth, under the Linux Foundation

How do I tell what version of Sawtooth is running?
--------------------------------------------------
::

    $ sawtooth --version
    sawtooth-cli (Hyperledger Sawtooth) version 1.0.4

What's the difference betweeen the ``sawtooth``, ``sawadm``, ``sawnet``, and ``sawset`` commands?
-------------------------------
``sawadm``
    Administration tasks such as creating the genesis batch file or validator key generation
``sawnet``
    Interact with Sawtooth network, such as comparing chains across nodes
``sawset``
    Change genesis block settings or views, create, and vote on new block proposals
``sawtooth``
    Interfact with a Sawtooth validator, such as batches, blocks, identity, keygen, peers, settings, state, and transaction information

Must software developed with Sawtooth be open source?
------------------------

IANAL, but no.  Sawtooth uses the `Apache 2 license, a permissive license,so can be used with both open or closed source

I get this error when running ``sawtooth setting list`` or ``xo list`` : ``Error 503: Service Unavailable``
-----------------------------

This usually occurs when there is no genesis node created.  To create, type the following:
::

    sawtooth keygen
    sawset genesis
    sudo -u sawtooth sawadm genesis config-genesis.batch

Then start the validator:
::

    sudo sawadm keygen
    sudo -u sawtooth sawtooth-validator -vv

How to I delete prevoiusly existing blockchain data?
----------------------------------

Type the following: ``sudo -u sawtooth rm -rf /var/lib/sawtooth/*``

I get a usage error running ``sawnet peers`` or ``sawnet list-blocks``
----------------------------------------------------

These commands were added after the Sawtooth 1.0.4 release and are not available yet.

© Copyright 2018, Intel Corporation.
