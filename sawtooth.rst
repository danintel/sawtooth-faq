Unofficial FAQ: Sawtooth in General
========================
[PREVIOUS_ | HOME_ | NEXT_]

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
Sawtooth Architecture
	https://sawtooth.hyperledger.org/docs/core/releases/latest/architecture.html
Sawtooth chat main channel (several others Sawtooth channels exist here)
    https://chat.hyperledger.org/channel/sawtooth
Official Sawtooth FAQ
    https://www.hyperledger.org/wp-content/uploads/2018/01/Hyperledger_Sawtooth_FAQ.pdf
Unofficial Sawtooth FAQ
    https://github.com/danintel/sawtooth-faq

Where are some good introductory videos?
---------------------------------------
* *Hyperledger Sawtooth 1.0: Market Significance & Technical Overview*
(Hyperledger, 2018, 61:27)
Free registration required
https://gateway.on24.com/wcc/gateway/linux/1101876/1585244/hyperledger-sawtooth-v10-market-significance-and-technical-overview
https://www.hyperledger.org/resources/webinars
* *Hyperledger Sawtooth 1.0 Architecture and App Development*
(Bitwise IO, 2018, 31:26)
https://www.youtube.com/watch?v=uBebFQM49Xk
* You can find several more at
https://www.youtube.com/results?search_query=Hyperledger+Sawtooth

What is the difference between Hyperledger and Sawtooth?
--------------------------------------------------------
* Sawtooth (or Hyperledger Sawtooth) is blockchain software sponsored by Intel Corporation, but does not require Intel hardware. See https://www.hyperledger.org/projects/sawtooth
* Hyperledger is a consortium that includes Sawtooth. "Hyperledger is an open source collaborative effort created to advance cross-industry blockchain technologies. It is a global collaboration, hosted by The Linux Foundation" See https://www.hyperledger.org/.

What is the difference between Hyperledger Sawtooth and Hyperledger Fabric?
-----------------------
Hyperledger Sawtooth and Fabric are two independent implementations of a blockchain under the Linux Foundation's Hyperledger Blockchain project.
Here are some differences:

* Fabric's Smart Contract must be written in GoLang or Javascript. Sawtooth transaction processors can be written in multiple languages, such as Rust, Python, Go, or JavaScript. SDKs for other languages are being added
* Fabric has "endorsing peers" and ordering services to pre-process transactions. Sawtooth has a validator that handles everything from validating the transactions and distributing the transaction to peer nodes
* Fabric stores data in a leveldb or couchdb, with a separate ledger per channel. Sawtooth stores all data in a central lmdb database with each transaction family using a separate address prefix.
* Fabric has multiple components, including Orderers, Peers, CAs, CouchDB, adn Tools. Sawtooth has the Sawtooth Validator and a Transaction Processor for each Transaction Family. The Validator's REST API communicates with a client
* Sawtooth is easier to use than Fabric (which needs a team to deploy)

Based on
https://www.skcript.com/svr/hyperledger-fabric-to-sawtooth

What differentiates Sawtooth from other blockchains?
-----------------------
This includes:

* State agreement, which assures each node has cryptographically-verifiable, identical copies of the blockchain
* Byzantine Fault Tolerant (BFT) consensus, through PoET
* Unpluggable consensus on-the-fly (without restarting)
* Multi-language SDK support (Python, Go, Javascript, Rust, with more being added)
* Parallel transaction processing

How does a blockchain differ from a database?
------------------------------
* A database has one master copy. A blockchain has multiple authoriative copies
* A database can be changed after a commit. A blockchain's records are immutable and cannot be undone after a commit
* A database must have a trusted central authority

Sawtooth is designed for permissioned and private blockchains. Can it work well for a public blockchain?
-------------------------------------------
Sawtooth would work for public blockchain, as well. The features we're providing in Sawtooth are designed for a permissioned, private network in mind. For a public blockchain, you probably want to use BFT consensus (such as PoET-SGX). There is no mining.

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
IANAL, but no. Sawtooth uses the Apache 2 license, a permissive license,so can be used with both open or closed source

I get this error when running ``sawtooth setting list`` or ``xo list`` : ``Error 503: Service Unavailable``
-----------------------------
This usually occurs when there is no genesis node created. To create, type the following:

::

    # Create the genesis node:
    sawtooth keygen
    sawset genesis
    sudo -u sawtooth sawadm genesis config-genesis.batch
    # Start the validator:
    sudo sawadm keygen
    sudo -u sawtooth sawtooth-validator -vv

I get this error when running ``sudo -u sawtooth sawadm genesis config-genesis.batch`` : ``Permission denied``
------------------------------------
The ownership or permission is wrong. To fix it, type:

::

    $ sudo chown sawtooth:sawtooth /var/lib/sawtooth
    $ sudo chmod 750 sawtooth:sawtooth /var/lib/sawtooth
    $ ls -ld /var/lib/sawtooth
    drwxr-x--- 2 sawtooth sawtooth 4096 Jun  2 14:43 /var/lib/sawtooth


How to I delete previously-existing blockchain data?
----------------------------------
Type the following: ``sudo -u sawtooth rm -rf /var/lib/sawtooth/*``

I get a usage error running ``sawnet peers`` or ``sawnet list-blocks``
----------------------------------------------------
These commands were added after the Sawtooth 1.0.4 release and are not available in earlier releases.

How do I detect a forked blockchain in a Sawtooth network?
-------------------------------------------------
Use `sawnet compare-chains` and look for a different set of block(s) at
the head of the chains.
This is distinct from the case where one node has a blockchain that's not
up-to-date, but has conflicting heads ("forked").
Forking can occur if the Sawtooth network is partitioned and cannot fully communicate.
It can also be the result of a bug in transaction processing
(for example, transactions don't serialize in a deterministic way).

How do I list and install Sawtooth packages?
--------------------------------------------
Here is how to setup the Sawtooth stable repository, list the packages,
and install the core packages
(sawtooth, python3-sawtooth-cli, python3-sawtooth-sdk, python3-sawtooth-signing):

::

    $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 8AA7AF1F1091A5FD
    $ sudo add-apt-repository 'deb http://repo.sawtooth.me/ubuntu/1.0/stable xenial universe'
    $ sudo apt update
    $ aptitude install sawtooth python3-sawtooth-*
    $ aptitude search sawtooth
    p  python3-sawtooth-block-info     - Sawtooth Block Info Transaction Processor 
    iA python3-sawtooth-cli            - Sawtooth CLI                              
    p  python3-sawtooth-config         - Sawtooth Config Transaction Processor
    p  python3-sawtooth-ias-client     - Sawtooth IAS Client 
    p  python3-sawtooth-ias-proxy      - Sawtooth IAS Proxy  
    c  python3-sawtooth-identity       - Sawtooth Identity Transaction Processor   
    iA python3-sawtooth-intkey         - Sawtooth Intkey Python Example            
    p  python3-sawtooth-manage         - Sawtooth Lake Management Library          
    iA python3-sawtooth-poet-cli       - Sawtooth PoET CLI                         
    iA python3-sawtooth-poet-common    - Sawtooth PoET Common Modules              
    iA python3-sawtooth-poet-core      - Sawtooth Core Consensus Module            
    iA python3-sawtooth-poet-families  - Sawtooth Transaction Processor Families   
    p  python3-sawtooth-poet-sgx       - Sawtooth PoET SGX Enclave                 
    iA python3-sawtooth-poet-simulator - Sawtooth PoET Simulator Enclave           
    iA python3-sawtooth-rest-api       - Sawtooth REST API                         
    i  python3-sawtooth-sdk            - Sawtooth Python SDK                       
    iA python3-sawtooth-settings       - Sawtooth Settings Transaction Processor   
    iA python3-sawtooth-signing        - Sawtooth Signing Library                  
    iA python3-sawtooth-validator      - Sawtooth Validator                        
    iA python3-sawtooth-xo             - Sawtooth XO Example                       
    i  sawtooth                        - Hyperledger Sawtooth Distributed Ledger   
    p  sawtooth-admin-tools            - Sawtooth Admin Tools                      
    BB sawtooth-cxx-sdk                - Hyperledger Sawtooth C++ SDK
    p  sawtooth-intkey-tp-go           - Sawtooth Intkey TP Go                     
    p  sawtooth-noop-tp-go             - Sawtooth Noop TP Go                       
    p  sawtooth-smallbank-tp-go        - Sawtooth Smallbank TP Go                  
    p  sawtooth-xo-tp-go               - Sawtooth Go XO TP

For more, up-to-date installation information see
https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/installation.html

I get this error installing ``sawtooth-cxx-sdk``: ``Depends: protobuf but it is not installable``
--------------------------------------------
The C++ SDK package is in the nightly repository.
Until the package dependency is fixed, here's a workaround to force an install:


::

    $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 44FC67F19B2466EA
    $ sudo apt-add-repository "deb [trusted=yes] http://repo.sawtooth.me/ubuntu/nightly xenial universe"
    $ sudo apt update
    $ apt download sawtooth-cxx-sdk
    $ sudo dpkg -i  sawtooth-cxx-sdk_1.1.1.dev808_amd64.deb
    $ pkg contents sawtooth-cxx-sdk

Does Hyperledger Composer support Sawtooth?
---------------------------
No.


What does this error mean ``repository ... xenial InRelease' doesn't support architecture 'i386'``?
---------------------------
You installed on a 32-bit-only system. Install on a 64-bit system.

I get this error running ``sawset``: ``ModuleNotFoundError: No module named 'colorlog'``
-------------------------------
Something went wrong with installing Python dependencies or they were removed.
In this case, install ``colorlog`` with ``sudo apt install python3-colorlog`` or with``pip3 install colorlog``

I get this error starting Sawtooth:
``lmdb.DiskError: /var/lib/sawtooth/poet-key-state-03efb2aa.lmdb: No space left on device``
-----------------------------
Besides the obvious problem of no disk space, it could be your OS or filesystem does not support sparse files.  The LMDB databases used by Sawtooth are 1TB sparse (mostly unallocated) files.

How do I report a bug?
---------------------------
Use the JIRA bug tracking system at
https://jira.hyperledger.org/projects/STL/issues/STL-51?filter=allopenissues
For security bugs only, send email to security@hyperledger.org


[PREVIOUS_ | HOME_ | NEXT_]

.. _PREVIOUS: README.rst
.. _HOME: README.rst
.. _NEXT: transaction-processing.rst

© Copyright 2018, Intel Corporation.
