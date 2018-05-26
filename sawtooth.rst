FAQ: Sawtooth in General
========================

What is Sawtooth?
-----------------
Hyperledger Sawtooth is a modular enterprise blockchain platform for building, deploying, and running distributed ledgers.
The design philosophy targets keeping ledgers distributed and making smart contracts safe, particularly for enterprise use.
Hyperledger Sawtooth includes a novel consensus algorithm, Proof of Elapsed Time (PoET), which targets large distributed validator populations with minimal resource consumption.
No special hardware is required to run Sawtooth or PoET.

What are some useful Sawtooth links?
------------------------------------

https://sawtooth.hyperledger.org/docs/core/nightly/master/introduction.html
    Sawtooth introduction
https://www.hyperledger.org/projects/sawtooth
    Sawtooth introduction and download
https://github.com/hyperledger/sawtooth-core
    Github repository for Sawtooth Core
https://sawtooth.hyperledger.org/docs/core/nightly/master/
    Sawtooth documentation, with several guides and references, including:
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide.html
    Sawtooth Application Developer's Guide
https://chat.hyperledger.org/channel/sawtooth
    Sawtooth chat main channel (several others Sawtooth channels exist here)

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

For more information, see the Sawtooth CLI Command Reference at https://sawtooth.hyperledger.org/docs/core/releases/latest/cli.html

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
