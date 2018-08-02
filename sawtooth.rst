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
GitHub repository for Sawtooth Core
    https://github.com/hyperledger/sawtooth-core
Sawtooth documentation, with several guides and references, including:
    https://sawtooth.hyperledger.org/docs/core/nightly/master/
Sawtooth Application Developer's Guide
    https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide.html
Sawtooth Architecture
    https://sawtooth.hyperledger.org/docs/core/releases/latest/architecture.html
Sawtooth chat main channel (several others Sawtooth channels exist here)
    https://chat.hyperledger.org/channel/sawtooth
Sawtooth mailing list (not as popular as the chat channel above)
    https://lists.hyperledger.org/g/sawtooth
Official Sawtooth FAQ
    https://www.hyperledger.org/wp-content/uploads/2018/01/Hyperledger_Sawtooth_FAQ.pdf
Unofficial Sawtooth FAQ
    https://github.com/danintel/sawtooth-faq

Where are some good introductory videos?
---------------------------------------
Hyperledger Sawtooth 1.0: Market Significance & Technical Overview (Hyperledger, 2018, 61:27) (free registration required):
  https://gateway.on24.com/wcc/gateway/linux/1101876/1585244/hyperledger-sawtooth-v10-market-significance-and-technical-overview
  https://www.hyperledger.org/resources/webinars
Hyperledger Sawtooth 1.0 Architecture and App Development (Bitwise IO, 2018, 31:26):
  https://www.youtube.com/watch?v=uBebFQM49Xk
You can find several more here:
  https://www.youtube.com/results?search_query=Hyperledger+Sawtooth

Are there any Sawtooth courses available?
-----------------------------------------
Yes.  EdX has a "Blockchain for Business" course that reviews Blockchain technology and includes an introduction to Sawtooth and other Hyperledger blockchain software.  See
https://www.edx.org/course/blockchain-business-introduction-linuxfoundationx-lfs171x-0
An intermediate course, edx @01 "Hyperledger Sawtooth for Application Developers" is under final review for release.

Are there any example applications based on Sawtooth?
-----------------------------------------------------
A simple application that implements a cookie jar showing just the Sawtooth API:
  https://github.com/danintel/sawtooth-cookiejar
A example application that implements a simple wallet application:
  https://github.com/askmish/sawtooth-simplewallet
A more complex example that implements a supply chain example and demonstrates many of the key concepts behind the implementation of a complete Sawtooth application:
  https://github.com/hyperledger/sawtooth-supply-chain
An example application that shows how to  exchange quantities of customized "Assets" with other users on the blockchain:
  https://github.com/hyperledger/sawtooth-marketplace

What is the difference between Hyperledger and Sawtooth?
--------------------------------------------------------
* Sawtooth (or Hyperledger Sawtooth) is a blockchain implementation initially contributed by Intel Corporation and now maintained by the Sawtooth community.  Sawtooth does not have to be deployed on Intel hardware; however, Sawtooth does include the optional PoET consensus module, which uses Intel SGX to provide an efficient, Byzantine Fault Tolerant consensus mechanism that does not rely on expensive and inefficient mining algorithms. See https://www.hyperledger.org/projects/sawtooth
* Hyperledger is a consortium that includes Sawtooth as well as other blockchain implementations. "Hyperledger is an open source collaborative effort created to advance cross-industry blockchain technologies. It is a global collaboration, hosted by The Linux Foundation" See https://www.hyperledger.org/.

What is the difference between Sawtooth, Sawtooth Lake, and Hyperledger Sawtooth?
-------------------------------
Sawtooth Lake was Intel's original code name for its blockchain research project, named after a lake in the Sawtooth Mountains of central Idaho. After it was contributed to the Linux Foundation's Hyperledger consortium, the name was changed to Hyperledger Sawtooth.  Sawtooth is just shorthand for Hyperledger Sawtooth and are the same thing.


What is the difference between Hyperledger Sawtooth and Hyperledger Fabric?
-----------------------
Hyperledger Sawtooth and Fabric are two independent implementations of a blockchain under the Linux Foundation's Hyperledger Blockchain project.
Here are some differences:

* Fabric's Smart Contract must be written in GoLang or Javascript. Sawtooth transaction processors can be written in multiple languages, such as Rust, Python, Go, or JavaScript. SDKs for other languages are being added
* Fabric has "endorsing peers" and ordering services to pre-process transactions. Sawtooth has a validator that handles everything from validating the transactions and distributing the transaction to peer nodes
* Fabric stores data in a leveldb or couchdb, with a separate ledger per channel. Sawtooth stores all data in a central lmdb database with each transaction family using a separate address prefix.
* Fabric has multiple components, including Orderers, Peers, CAs, CouchDB, and Tools. Sawtooth has the Sawtooth Validator and a Transaction Processor for each Transaction Family. The Validator's REST API communicates with a client

Based on
https://www.skcript.com/svr/hyperledger-fabric-to-sawtooth

What differentiates Sawtooth from other blockchains?
-----------------------
This includes:

* State agreement, which assures each node has cryptographically-verifiable, identical copies of the blockchain
* novel Byzantine Fault Tolerant (BFT) consensus, through PoET
* Unpluggable consensus on-the-fly (without restarting)
* Multi-language SDK support (Python, Go, Javascript, Rust, with more being added)
* Parallel transaction processing

For more on Sawtooth differentiation and philosophy, see
https://www.hyperledger.org/blog/2016/11/02/meet-sawtooth-lake

Should I use Sawtooth or other blockchain software for my application?
---------------------------------------
You should look for existing blockchain platforms that will fit your use case, sort them out by features, maturity (are they production ready?), and community support.  We hope Sawtooth fits your needs.

How does a blockchain differ from a database?
------------------------------
* A database has one master copy. A blockchain has multiple authoritative copies
* A database can be changed after a commit. A blockchain's records are immutable and cannot be undone after a commit
* A database must have a trusted central authority

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
IANAL; however, Sawtooth is released under the Apache 2 license, a permissive license, and so should be able to be used in both open and closed source applications.

Can I copy a Sawtooth Core source file to include with my project?
-----------------------------------
Yes, if you follow the Apache 2 license terms, which include requiring preserving copyright and license notices.
Sawtooth depends on other runtime software that has separate terms.

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

What does ``Failed to reach common ancestor`` mean from ``sawnet compare-chains``?
--------------------------
It means the blockchains have no blocks in common, including the genesis block.  This usually happens when a second node is added with its own genesis node.  Only the first node in a Sawtooth network should be created with a genesis block.

Does Hyperledger Composer support Sawtooth?
---------------------------
No, not now.

Does Hyperledger Explorer support Sawtooth?
----------------------------------
No, not now. There is a Sawtooth Explorer at
https://github.com/hyperledger/sawtooth-explorer
It may or may not be merged with Hyperledger Explorer in the future.
Sawtooth Explorer provides visibility into the Sawtooth blockchain for node operators.

How do I report a bug?
---------------------------
Use the JIRA bug tracking system at
https://jira.hyperledger.org/projects/STL/issues/STL-51?filter=allopenissues
For security bugs only, send email to security@hyperledger.org

Can you explain Global State with an example?
----------------------------------------------
Global state is where sawtooth and TPs read/write blockchain data. Examples are a-plenty if you look at the github repo examples (intkey, XO, etc.)
The "state" is implemented as a Radix Merkle Trie over the LMDB database, where the 'keys' are 35 bytes (70 characters) and the scheme for the keys is up to the TP developer.  The first 3 bytes (6 chars) of the key identifies a unique TP namespace and it is recommended to avoid colliding with other TP namespaces.
To enable your TP to read/write (or in context parlance "get/set") data at addresses, you need to specify those addresses *a priori* in the Transaction inputs/outputs. Otherwise you will get Authorization errors. The addresses your TP will read or write to need to be deterministic.

Using the SimpleWallet application as an example (see example application links above), the blockchain will contain transactions showing deposits, withdrawals and transfers between accounts. The global state will contain the balance in the different accounts corresponding at the current point in time, after all transactions in the chain have been processed.

What is the difference between the Merkle Radix Trie and the blockchain?
-----------------------------
The blockchain itself just stores transactions, not state, so reading the data in the last block does not say much by itself. Data in the blockchain is also immutable and can never change (except by adding new blocks). The radix trie is a different data structure that is used to make fast queries to the state. The root of the Merkle Trie is a hash. One can easily identify if something changed when the root hash changes. The Merkle Trie addressing allows quick retrieval at an address and partial queries of address prefixes.

Are 32-byte IDs within a transaction family large enough to avoid collisions?
-------------------------------------
Yes. If they are being generated with a random distribution, the chances are vanishingly rare. A UUID is only 16-bytes and if you generated a billion per second, it would take 100 years before you would expect 50% odds of a collision.

Why is Sawtooth capable of supporting large network populations of nodes?
--------------------------
One of the reasons is the homogeneous nature of Sawtooth Nodes. You don't have different nodes with specialized functions, so it's easy to setup and manage many nodes. Secondly, and more importantly, the PoET consensus mechanism has been designed for large networks. It's not very efficient in small networks and you'll likely get much better performance with other mechanisms in a small network, but PoET handles large populations easily.

Is there a Sawtooth security evaluation?
-----------------------------
Yes. This was required to be a part of the Linux Foundation's Hyperledger project.  See
https://www.hyperledger.org/blog/2018/05/22/hyperledger-sawtooth-security-audit

Does Sawtooth restore state when a peer restarts or when a peer is out-of-sync with the network?
--------------------
Yes.


When content at an address is changed several times by the transactions in a block, what appears in the state (Merkle Tree)?
-----------------------------
The only thing that hits state is the aggregate (final) set of address changes due to the transactions in the block. If multiple transactions in a single block modify an address, there will only be one 'set'. You could see the transaction level changes in the receipts if you needed to.

In order to create a Sawtooth application, do I need to clone and modify the entire ``sawtooth-core`` repository?
-----------------------
No. It can be done that way, but it's not recommended.
All you need to write is the client application and the Transaction Processor.
The core Sawtooth functionality should be installed as packages instead of being built from source and integrated with your application.
Here's some simple sample applications that are in standalone source repositories:

* Simple Wallet, https://github.com/askmish/sawtooth-simplewallet
* Cookie Jar, https://github.com/danintel/sawtooth-cookiejar
* Cryptomoji,  https://github.com/hyperledger/education-cryptomoji
* Simple Supply Chain, https://github.com/hyperledger/education-sawtooth-simple-supply

What is Sawtooth *global state agreement*?
--------------------------------------------
Sawtooth writes state to a verifiable structure called a *Radix Merkle Trie* and the verification part (the root hash) is included in the consensus process. That means that agreement is not just on the ordering of transactions but also on the resulting contents of the entire database.

This guards against a variety of possible failures during the application of a transaction (e.g. different library version installed, a write failure, a local database corruption, numerical representation differences).

Of course the feature is mainly targeted at protecting the integrity of a production network, but it is also helpful during development. Running applications over test networks can help identify nondeterminism and that will only be apparent if you form consensus over state.

How can CPU vulnerabilities such as Spectre and Meltdown impact Sawtooth?
-----------------------------------
Sawtooth is a CPU-agnostic blockchain platform. It includes an optional TEE/SGX feature which enhances BFT protections for PoET.  PoET is designed following a defense-in-depth approach. There are three or so mechanisms that work in different aspects of the protocol independently from the TEE. This includes three tests performed by PoET:

* c-test: A node must wait c blocks after admission before its blocks will be accepted - this is to prevent trying to game identities and some obscure corner scenarios.
* K-test: The node can publish at most K blocks before its peers require it to recertify itself.
* z-test: And perhaps most importantly a node may not publish at frequency greater than z

Finally, should a node run a compromised consensus protocol, the main characteristic at risk would be *fairness*. It would not be able to impact *correctness* network-wide. That is, it cannot publish invalid transactions. If it does the other nodes will just reject those transactions and the associated block(s) and they will not commit network-wide.

Are Docker containers required to run Sawtooth?
--------------------------
Docker is a quick and easy way to get Sawtooth up and running.
However, unlike other Hyperledger ledgers, Sawtooth does not require Docker.
Follow the instructions to run on Ubuntu at
https://sawtooth.hyperledger.org/docs/core/releases/latest/app_developers_guide/ubuntu.html
For specific apps, you can run without docker by manually running commands in a ``Dockerfile`` as follows:

* Install Sawtooth on an Ubuntu following the instructions in the *Sawtooth Applications Developer's Guide*
* Create the Genesis Block. See Guide in previous step
* Install required packages listed under the RUN line in the ``Dockerfile`` for each container
* Install your application's transaction processor and client.
* Make sure your client app connects to the REST API at ``http://localhost:8008`` instead of ``http://rest-api:8008``
* Make sure your transaction processor connects to ``tcp://localhost:4004`` instead of ``tcp://validator:4004``
* Start the Validator, REST API, and Settings TP:
  ``sudo -u sawtooth sawtooth-validator -vv &``
  ``sudo -u sawtooth sawtooth-rest-api -vvv &``
  ``sudo -u sawtooth settings-tp -vv &``

* Start your application-specific transaction processor(s). See the ``CMD`` line in the ``Dockerfile`` for your TP
* Start your application client (see ``CMD`` in your client ``Dockerfile``)


[PREVIOUS_ | HOME_ | NEXT_]

.. _PREVIOUS: README.rst
.. _HOME: README.rst
.. _NEXT: installation.rst

© Copyright 2018, Intel Corporation.
