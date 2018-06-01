Preliminary FAQ: Sawtooth in General
========================
[`PREVIOUS`_ | `HOME`_ | `NEXT`_]

.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

What is Sawtooth?
-----------------
Hyperledger Sawtooth is a modular enterprise blockchain platform for building, deploying, and running distributed ledgers.
The design philosophy targets keeping ledgers distributed and making smart contracts safe, particularly for enterprise use.
Hyperledger Sawtooth includes a novel consensus algorithm, Proof of Elapsed Time (PoET), which targets large distributed validator populations with minimal resource consumption.
No special hardware is required to run Sawtooth or PoET.

What are some useful Sawtooth links?
------------------------------------

Sawtooth introduction
    https://sawtooth.hyperledger.org/docs/core/nightly/master/introduction.html
Sawtooth introduction and download
    https://www.hyperledger.org/projects/sawtooth
Github repository for Sawtooth Core
    https://github.com/hyperledger/sawtooth-core
Sawtooth documentation, with several guides and references, including:
    https://sawtooth.hyperledger.org/docs/core/nightly/master/
Sawtooth Application Developer's Guide
    https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide.html
Sawtooth chat main channel (several others Sawtooth channels exist here)
    https://chat.hyperledger.org/channel/sawtooth
Sawtooth FAQ
    https://github.com/danintel/sawtooth-faq

What is the difference between Hyperledger and Sawtooth?
--------------------------------------------------------

* Sawtooth (or Hyperledger Sawtooth) is blockchain software sponsored by Intel Corporation, but does not require Intel hardware.

* Hyperledger is a consortium of Linux blockchain software, including Sawtooth, under the Linux Foundation

Sawtooth is designed for permissioned and private blockchains. Can it work well for a public blockchain?
-------------------------------------------
Sawtooth would work for public blockchain, as well. The features we're providing in Sawtooth are designed for a permissioned, private network in mind.  For a public blockchain, you probably want to use BFT consensus (such as PoET-SGX). There is no mining.

How do I tell what version of Sawtooth is running?
--------------------------------------------------
::

    $ sawtooth --version
    sawtooth-cli (Hyperledger Sawtooth) version 1.0.4

What's the difference between the ``sawtooth``, ``sawadm``, ``sawnet``, and ``sawset`` commands?
-------------------------------
``sawadm``
    Administration tasks such as creating the genesis batch file or validator key generation
``sawnet``
    Interact with Sawtooth network, such as comparing chains across nodes
``sawset``
    Change genesis block settings or views, create, and vote on new block proposals
``sawtooth``
    Interact with a Sawtooth validator, such as batches, blocks, identity, keygen, peers, settings, state, and transaction information

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

How to I delete previously-existing blockchain data?
----------------------------------

Type the following: ``sudo -u sawtooth rm -rf /var/lib/sawtooth/*``

I get a usage error running ``sawnet peers`` or ``sawnet list-blocks``
----------------------------------------------------

These commands were added after the Sawtooth 1.0.4 release and are not available in earlier releases.

How do I list and install Sawtooth packages?
--------------------------------------------
See https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/installation.html

Here is how to setup the stable repository and list the packages:

::

    $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 8AA7AF1F1091A5FD
    $ sudo add-apt-repository 'deb http://repo.sawtooth.me/ubuntu/1.0/stable xenial universe'
    $ sudo apt update
    $ aptitude  search python3-sawtooth-*
    p python3-sawtooth-block-info   - Sawtooth Block Info Transaction Processor
    p python3-sawtooth-cli          - Sawtooth CLI
    p python3-sawtooth-ias-client   - Sawtooth Intel Attestation Service Client
    p python3-sawtooth-ias-proxy    - Sawtooth Intel Attestation Service Proxy
    p python3-sawtooth-identity     - Sawtooth Identity Transaction Processor
    p python3-sawtooth-intkey       - Sawtooth Intkey Python Example
    p python3-sawtooth-manage       - Sawtooth Lake Management Library
    p python3-sawtooth-poet-cli     - Sawtooth PoET CLI
    p python3-sawtooth-poet-common  - Sawtooth PoET Common Modules
    p python3-sawtooth-poet-core    - Sawtooth Core Consensus Module
    p python3-sawtooth-poet-families  - Sawtooth Transaction Processor Families
    p python3-sawtooth-poet-sgx       - Sawtooth PoET SGX Enclave
    p python3-sawtooth-poet-simulator - Sawtooth PoET Simulator Enclave
    p python3-sawtooth-rest-api       - Sawtooth REST API
    p python3-sawtooth-sdk            - Sawtooth Python SDK
    p python3-sawtooth-settings       - Sawtooth Settings Transaction Processor
    p python3-sawtooth-signing        - Sawtooth Signing Library
    p python3-sawtooth-validator      - Sawtooth Validator
    p python3-sawtooth-xo             - Sawtooth XO Example
    $ sudo apt install sawtooth



[`PREVIOUS`_ | `HOME`_ | `NEXT`_]

.. _PREVIOUS: README.md
.. _HOME: README.md
.. _NEXT: transaction-processing.rst

© Copyright 2018, Intel Corporation.
