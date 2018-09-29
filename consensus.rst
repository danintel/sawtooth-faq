Unofficial FAQ: Sawtooth Consensus Algorithms (including PoET)
====================
[PREVIOUS_ | HOME_ | NEXT_]

.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

What consensus algorithms does Sawtooth support?
-------------------
dev-mode
    Only suitable for testing TPs with single validator deployments.  Uses a simplified random-leader algorithm for development and testing.  Not for production use
PoET Simulator
    Simulates the SGX environment, and provides CFT similar to Fabric and some other blockchains.  Requires poet-validator-registry TP. Runs on any processor (does not use SGX).  Has Crash Fault Tolerance and can be used for production networks
PoET SGX
    Takes advantage of SGX in order to provide consensus with Byzantine Fault Tolerance (BFT), like PoW algorithms have, but at very low CPU usage. PoET SGX is the only algorithm that has hardware requirements (a processor supporting SGX)
RAFT
    Consensus algorithm that elects a leader for a term of arbitrary time. Leader replaced if it times-out. Raft is faster than PoET, but is CFT, not BFT. Also Raft does not fork.  For Sawtooth RAFT is new and still being stabilized.

Will Sawtooth support more consensus algorithms in the future?
------------------------------------------
Yes. With pluggable consensus, the idea is to have a meaningful set of consensus algorithms so the "best fit" can be applied to an application's use case.  RAFT is a recent addition--still being stabilized. There is a PBFT prototype in the works.  Others are being planned.

REMME.io has independently implemented Algorand Byzantine Agreement on Sawtooth.

Where is Raft documented?
------------------------
https://sawtooth.hyperledger.org/docs/raft/nightly/master/
To use, basically set ``sawtooth.consensus.algorithm`` to ``raft`` and
``sawtooth.consensus.raft.peers`` to a list of peer nodes (network public keys).


Does the PoET Simulator implement the same consensus algorithm as PoET SGX?
------------------------------
Yes--they are same same consensus algorithm. The difference is the
PoET Simulator also simulates SGX hardware, allowing PoET to run on non-SGX
hardware.

Is PoET Simulator suitable for production use?
----------------------
Yes.  It is for systems that do not have SGX and is intended for use in production.

What is unpluggable consensus?
-------------------
Sawtooth supports unpluggable consensus, meaning you can change the consensus algorithm on the fly,
at a block boundary.
Changing consensus on the fly means it is done without stopping validators, flushing state,
or starting over with a new genesis block.
It is also called Dynamic Consensus.

Can my Sawtooth network have validators with a mixture of SGX PoET or Simulator PoET?
------------------------------------------
No. You need to pick one consensus for all nodes.
But you can change consensus after the network has started.

Can I use the PoET simulator instead of PoET SGX for production?
------------------------------
Yes, PoET simulator is for production use, not just for development. Both PoET Simulator and SGX have tests to guard against bad actors, such as the "Z Test" to check a validator is not winning too frequently.
PoET Simulator simulates the SGX environment and provides CFT (similar to Fabric and other blockchain software), which is good enough to go into production.
That said, PoET SGX is preferred because of the additional SGX protections for generating the wait time.

What cloud services offer SGX?
------------------------------
SGX is available on IBM cloud and Alibaba.
Early access was available on Microsoft Azure, but not now.

Does PoET SGX function with SGX on cloud services?
---------------------------------------------
No. For PoET SGX to function, one also needs Platform Services (PSW), which is not available from any cloud provider.
Instead, one can use PoET Simulator, which is also production-ready.
But other software software that requires SGX may be deployed on cloud services.

I get this error during PoET SGX registration: "Machine requires update (probably BIOS) for SGX compliance."
-------------------
During EPID provisioning your computer is trying to get an anonymous credential from Intel. If that process is failing one possibility is that there's a network issue like a proxy. A second possibility is that there's some firmware out of date and so the protocol isn't doing what the backend expects it to. You can check for a firmware / BIOS update for that platform.

SGX also needs to be enabled in the BIOS menu.

Does Sawtooth require a certain processor to be deployed on a network?
-------------------
No.  If you use PoET SGX consensus you need a processor that supports SGX.

Does Sawtooth require SGX?
-------------------
No.  SGX is only needed if you use the hardened version of PoET, PoET SGX.
We also have a version of PoET that just uses conventional software, PoET Simulator,
which runs on a Sawtooth network with any processor.

How is PoET duration time computed?
------------------------
It is ``duration = random_float(0,1) * local_mean_wait_time``

Why does PoET use exponentially-distributed random variable function instead of a uniform function?
------------------------------------
That is to minimize the number of "collisions" in the distribution of a given round of wait timers generated by the population,
where "collision" means two or more timers that are near the minimum of the distribution and within some latency threshold.
The distribution of the random function is shaped by a population estimate of the network, which is determined by examining the last N blocks.
In an ideal world, you want a distribution where one and only one random wait time is around the desired inter block duration, and then there is a decent sized gap.

Where is PoET 1.0 Specification?
----------------------------------
https://sawtooth.hyperledger.org/docs/core/releases/1.0/architecture/poet.html

Where is the PoET SGX Enclave configuration file?
----------------------
It is at ``/etc/sawtooth/poet_enclave_sgx.toml`` .
It is only for configuring PoET SGX Enclave, not the PoET simulator (without SGX).
A sample file is at
https://github.com/hyperledger/sawtooth-poet/blob/master/sgx/packaging/poet_enclave_sgx.toml.example
The configuration is documented at
https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/configuring_sawtooth/poet_sgx_enclave_configuration_file.html

I run ``sudo -u sawtooth poet registration create . . .`` and get ``Permission denied: 'poet_genesis.batch'`` error
-----------------------------------------
Change to a sawtooth user-writable directory before running the command: ``cd /tmp``


What does ``Consensus not ready to build candidate block`` mean?
---------------------------------
This message is usually an innocuous information message. It usually means that the validator isn't yet registered in the validator registry or that its previous registration has expired and it's waiting for the new one to commit.
The message occurs after the block publisher polls the consensus interface asking if it is time to build the block. If not enough time has elapsed, it logs that message.

However, if that message is rampant in the logs on all but one node, that might mean that none of them can register (they are deadlocked when launching a network). There's a few things that can cause that.

Unlikely but worth mentioning: are you mapping volumes into the containers? If all the validators are trying to use the same data file that would be bad. That would not happen unless all the nodes are on the same host.

More commonly, the defense-in-depth checks are too stringent during the initial launch. You can relax these parameters (see Settings_ in this FAQ) or, easier yet, relaunch the network.

How do I change the Sawtooth consensus algorithm?
---------------------------
* Start any consensus-required TPs, if any, on all nodes (for example
PoET requires the ``sawtooth_validator_registry`` TP).

* Use the ``sawset proposal create`` subcommand to modify ``sawtooth.consensus.algorithm`` (along with any consensus-required settings).  For an example, see https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/creating_sawtooth_network.html

The initial default consensus algorithm is ``devmode``, which is not for production use.

Here is an example that changes the consensus to Raft:
  ``sawset proposal create --url http://localhost:8008 --key /etc/sawtooth/keys/validator.priv  \
  sawtooth.consensus.algorithm=raft sawtooth.consensus.raft.peers=\
  '["0276f8fed116837eb7646f800e2dad6d13ad707055923e49df08f47a963547b631", \
  "035d8d519a200cdb8085c62d6fb9f2678cf71cbde738101d61c4c8c2e9f2919aa"]'``

Where can I find information on the proposed PoET2 algorithm?
------------------------------------
PoET2 is different from PoET in that it supports SGX without relying on Intel Platform Services Enclave (PSE), making it suitable in cloud environments.
PoET2 no longer saves anything across reboots (such as the clock, monotonic counters, or a saved ECDSA keypair).
The PoET2 SGX enclave still generates a signed, random duration value.
More details and changes are documented in the PoET2 RFC at
https://github.com/hyperledger/sawtooth-rfcs/pull/20/files
A video presentation (2018-08-23) is at
https://drive.google.com/drive/folders/0B_NJV6eJXAA1VnFUakRzaG1raXc
(starting at 7:45)

Where is the Consensus Engine API documented?
---------------------------------------------
At https://github.com/hyperledger/sawtooth-rfcs/pull/4
See also the "Sawtooth Consensus Engines" video at
20180426-sawtooth-tech-forum.mp4, starting at 10:00,
in directory
https://drive.google.com/drive/folders/0B_NJV6eJXAA1VnFUakRzaG1raXc

What are the minimum number of nodes needed for PoET?
------------------------------------------
PoET needs at least 3 nodes, but works best with at least 4 or 5 nodes. This is to avoid Z Test failures (a node winning too frequently).  In production, to keep a blockchain safe, more nodes are always better, regardless of the consensus. 10 nodes are good for internal testing. For production, have 2 nodes per identity.


[PREVIOUS_ | HOME_ | NEXT_]

.. _PREVIOUS: validator.rst
.. _HOME: README.rst
.. _NEXT: client.rst
.. _Settings: settings.rst

© Copyright 2018, Intel Corporation.
