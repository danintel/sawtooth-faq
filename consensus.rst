Sawtooth FAQ: Consensus Algorithms (including PoET)
===================================================

[PREVIOUS_ | FAQ_ | NEXT_]

.. contents::


What consensus algorithms does Sawtooth support?
------------------------------------------------
dev-mode
    Only suitable for testing TPs with single validator deployments. Uses a simplified random-leader algorithm for development and testing. Not for production use
PoET CFT (also known as PoET Simulator)
    PoET with a simulated SGX environment. Provides CFT similar to Fabric and some other blockchains. Requires poet-validator-registry TP. Runs on any processor (does not require Intel or SGX). Has Crash Fault Tolerance and can be used for production networks if BFT is not required
PoET SGX
    Takes advantage of SGX in order to provide consensus with Byzantine Fault Tolerance (BFT), like PoW algorithms have, but at very low CPU usage. PoET SGX is the only algorithm that has hardware requirements (a processor supporting SGX)
Raft
    Consensus algorithm that elects a leader for a term of arbitrary time. Leader replaced if it times-out. Raft is faster than PoET, but is CFT, not BFT. Also Raft does not fork. For Sawtooth Raft is new and still being stabilized.

Will Sawtooth support more consensus algorithms in the future?
--------------------------------------------------------------
Yes. With pluggable consensus, the idea is to have a meaningful set of consensus algorithms so the "best fit" can be applied to an application's use case. Raft is a recent addition--still being stabilized. There is a PBFT prototype in the works. Others are being planned.

REMME.io has independently implemented Algorand Byzantine Agreement on Sawtooth.

Where is Raft documented?
-------------------------
https://sawtooth.hyperledger.org/docs/raft/nightly/master/
To use, basically set ``sawtooth.consensus.algorithm`` to ``raft`` and
``sawtooth.consensus.raft.peers`` to a list of peer nodes (network public keys).

Does the PBFT implementation follow the original paper?
-------------------------------------------------------
Yes, it follows the original 1999 Castro and Liskov paper without modifications or optimizations.

Does the PoET CFT implement the same consensus algorithm as PoET SGX?
---------------------------------------------------------------------
Yes--they are same same consensus algorithm. The difference is the
PoET CFT also simulates the enclave module, allowing PoET to run on non-SGX
hardware.

For PoET CFT (PoET Simulator), should I generate my own ``simulator_rk_pub.pem`` file or do I use the one in ``/etc/sawtooth/`` ?
---------------------------------------------------------------------------------------------------------------------------------
No, you use the one that is installed. It must match the private key that is in the PoET Simulator. The public key is needed to verify attestation verification reports from PoET.

What is unpluggable consensus?
------------------------------
Sawtooth supports unpluggable consensus, meaning you can change the consensus algorithm on the fly,
at a block boundary.
Changing consensus on the fly means it is done without stopping validators, flushing state,
or starting over with a new genesis block.
It is also called Dynamic Consensus.

Can my Sawtooth network have validators with a mixture of PoET SGX and PoET CFT?
--------------------------------------------------------------------------------
No. You need to pick one consensus for all nodes.
But you can change consensus after the Sawtooth network has started.

Is PoET CFT suitable for production use?
----------------------------------------
Yes. It is for systems that do not have SGX and is intended for use in production. Both PoET CFT and PoET SGX have tests to guard against bad actors, such as the "Z Test" to check a validator is not winning too frequently.
PoET CFT simulates the SGX environment and provides CFT (similar to Fabric and other blockchain software), which is good enough to go into production.
That said, PoET SGX is preferred because of the additional SGX protections for generating the wait time.

What cloud services offer SGX?
------------------------------
SGX is available on IBM cloud and Alibaba.
Early access was available on Microsoft Azure, but not now.

Does PoET SGX function with SGX on cloud services?
--------------------------------------------------
No. For PoET SGX to function, one also needs Platform Services (PSW), which is not available from any cloud provider.
Instead, one can use PoET CFT, which is also supported.
But other software software that requires SGX may be deployed on cloud services.

I get this error during PoET SGX registration: "Machine requires update (probably BIOS) for SGX compliance."
------------------------------------------------------------------------------------------------------------
During EPID provisioning your computer is trying to get an anonymous credential from Intel. If that process is failing one possibility is that there's a network issue like a proxy. A second possibility is that there's some firmware out of date and so the protocol isn't doing what the backend expects it to. You can check for a firmware / BIOS update for that platform.

SGX also needs to be enabled in the BIOS menu.

Does Sawtooth require a certain processor to be deployed on a network?
----------------------------------------------------------------------

No. If you use PoET SGX consensus you need a processor that supports SGX.

Does Sawtooth require SGX?
--------------------------

No. SGX is only needed if you use the hardened version of PoET, PoET SGX.
We also have a version of PoET that just uses conventional software, PoET CFT,
which runs on a Sawtooth network with any processor.

How is PoET duration time computed?
-----------------------------------

It is ``duration = random_float(0,1) * local_mean_wait_time``

Why does PoET use exponentially-distributed random variable function instead of a uniform function?
---------------------------------------------------------------------------------------------------

That is to minimize the number of "collisions" in the distribution of a given round of wait timers generated by the population,
where "collision" means two or more timers that are near the minimum of the distribution and within some latency threshold.
The distribution of the random function is shaped by a population estimate of the network, which is determined by examining the last N blocks.
In an ideal world, you want a distribution where one and only one random wait time is around the desired inter block duration, and then there is a decent sized gap.

Where is PoET 1.0 Specification?
--------------------------------
https://sawtooth.hyperledger.org/docs/core/releases/latest/architecture/poet.html
Why is PoET SGX Byzantine Fault Tolerant?
-----------------------------------------
Because the PoET waiting time is enforced with an SGX enclave. There is also more defense-in-depth checks, but that doesn't make it BFT. In comparison, Bitcoin's PoW accomplishes the same thing with repeatedly hashing, which is effectively the same thing (although more wasteful) than PoET's trusted timer. For details, see the PoET 1.0 spec in the link above.

Where is the PoET SGX Enclave configuration file?
-------------------------------------------------
It is at ``/etc/sawtooth/poet_enclave_sgx.toml`` .
It is only for configuring PoET SGX Enclave, not the PoET CFT (PoET without SGX).
A sample file is at
https://github.com/hyperledger/sawtooth-poet/blob/master/sgx/packaging/poet_enclave_sgx.toml.example
The configuration is documented at
https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/configuring_sawtooth/poet_sgx_enclave_configuration_file.html

I run ``sudo -u sawtooth poet registration create . . .`` and get ``Permission denied: 'poet_genesis.batch'`` error
-------------------------------------------------------------------------------------------------------------------
Change to a sawtooth user-writable directory before running the command and make sure file `poet_genesis.batch` does not already exist: ``cd /tmp; ls poet_genesis.batch``


What does ``Consensus not ready to build candidate block`` mean?
----------------------------------------------------------------
This message is usually an innocuous information message. It usually means that the validator isn't yet registered in the validator registry or that its previous registration has expired and it's waiting for the new one to commit.
The message occurs after the block publisher polls the consensus interface asking if it is time to build the block. If not enough time has elapsed, it logs that message.

However, if that message is rampant in the logs on all but one node, that might mean that none of them can register (they are deadlocked when launching a network). There's a few things that can cause that.

Unlikely but worth mentioning: are you mapping volumes into the containers? If all the validators are trying to use the same data file that would be bad. That would not happen unless all the nodes are on the same host.

More commonly, the defense-in-depth checks are too stringent during the initial launch. You can relax these parameters (see Settings_ in this FAQ) or, easier yet, relaunch the network.

What does `` Failed to create wait certificate: Cannot create wait certificate because timer has timed out`` mean?
------------------------------------------------------------------------------------------------------------------
It means too much time has elapsed between the creation of the wait timer and the attempt to finalize the block and create the wait certificate.
Look at the logs for that node and determine when it started to publish the block prior to that error, and see what transpired in between. When the timer expires, the validator is supposed to wrap up the schedule immediately and create the block, so that message is kind of unusual.  In versions of Sawtooth before 1.0, we waited until the entire schedule executed, which could be quite long running, and this message was quite common.

How do I change the Sawtooth consensus algorithm?
-------------------------------------------------
* Install the software package containing the consensus engine you wish to use on all nodes, if it is not already installed.
* Start any consensus-required TPs, if any, on all nodes (for example PoET requires the ``sawtooth_validator_registry`` TP).
* Use the ``sawset proposal create`` subcommand to modify ``sawtooth.consensus.algorithm`` (along with any consensus-required settings). For an example, see https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/creating_sawtooth_network.html

The initial default consensus algorithm is ``devmode``, which is not for production use.

Here is an example that changes the consensus to Raft:
  ``sawset proposal create --url http://localhost:8008 --key /etc/sawtooth/keys/validator.priv  \
  sawtooth.consensus.algorithm=raft sawtooth.consensus.raft.peers=\
  '["0276f8fed116837eb7646f800e2dad6d13ad707055923e49df08f47a963547b631", \
  "035d8d519a200cdb8085c62d6fb9f2678cf71cbde738101d61c4c8c2e9f2919aa"]'``

How do I change the consensus algorithm for a network that has forked?
----------------------------------------------------------------------
Bring the network down to one node with the preferred blocks and submit
your consensus change proposal. Bring in the other nodes, with any consensus-required TPs running (for example, PoET requires the Validator Registry TP).

Where can I find information on the proposed PoET2 algorithm?
-------------------------------------------------------------

PoET2 is different from PoET in that it supports SGX without relying on Intel Platform Services Enclave (PSE), making it suitable in cloud environments.
PoET2 no longer saves anything across reboots (such as the clock, monotonic counters, or a saved ECDSA keypair).
The PoET2 SGX enclave still generates a signed, random duration value.
More details and changes are documented in the PoET2 RFC at
https://github.com/hyperledger/sawtooth-rfcs/pull/20/files
A video presentation (2018-08-23) is at
https://drive.google.com/drive/folders/0B_NJV6eJXAA1VnFUakRzaG1raXc
(starting at 7:45)

What is the Intel Platform Developers Kit for Blockchain - Ubuntu?
------------------------------------------------------------------

The PDK is a small form factor computer with SGX with Ubuntu, Hyperledger Sawtooth, and development software pre-installed. For information, see
https://designintools.intel.com/Intel_Platform_Developers_Kit_for_Blockchain_p/q6uidcbkcpdk.htm

Where is the Consensus Engine API documented?
---------------------------------------------

At https://github.com/hyperledger/sawtooth-rfcs/pull/4
See also the "Sawtooth Consensus Engines" video at
20180426-sawtooth-tech-forum.mp4, starting at 10:00,
in directory
https://drive.google.com/drive/folders/0B_NJV6eJXAA1VnFUakRzaG1raXc

What are the minimum number of nodes needed for PoET?
-----------------------------------------------------

PoET needs at least 3 nodes, but works best with at least 4 or 5 nodes. This is to avoid Z Test failures (a node winning too frequently). In production, to keep a blockchain safe, more nodes are always better, regardless of the consensus. 10 nodes are good for internal testing. For production, have 2 nodes per identity.

Can PoET be configured for small networks?
------------------------------------
Yes, for development purposes.
For production purposes, consider using another consensus algorithm.
For example, Raft or PBFT handles a small number of nodes nicely.
For PoET in a small blockchain network, disable defense-in-depth tests
for small test networks (say, < ~12 nodes) with:

::

    sawtooth.poet.block_claim_delay=1
    sawtooth.poet.key_block_claim_limit= 100000
    sawtooth.poet.ztest_minimum_win_count=999999999


How should peer nodes be distributed?
-------------------------------------

Blockchain achieves fault tolerance by having its state (data) completely duplicated among the peer nodes. Best practice means distributing your nodes--geographically and organizationally.
Distributing nodes on virtual machines sharing the same host does nothing to guard against hardware faults.
Distributing nodes at the same site does not protect against site outages.

Can I restrict what validator nodes win consensus?
--------------------------------------------------
No. Every peer node validates blocks and every peer node can publish a block.
You can write your own plugin consensus module to restrict what peer nodes win. Or modify an existing consensus module to experiment.

How do I restart a consensus engine?
------------------------------------
First stop the validator, then restart the consensus engine.
If you leave the validator engine running, it will not connect to the restarted consensus engine.

Do I start the consensus engine before or after the validator?
--------------------------------------------------------------
The consensus engine can start before or after the validator.
The preferred order is to start the validator first, then the consensus engine.
If you start the consensus engine before the validator, the consensus engine will retry connecting to the validator (through TCP port 5050) until it the consensus engine is successful.

What can cause a fork with PoET?
--------------------------------
In PoET, forks occur due to a network partition, the size of the network, the time it takes to transfer and validate blocks across the network, and the likelihood that two or more validator will think they have “won” and therefore publish a block during this time period.

TPs don’t really affect forks, unless they have a severe impact on the validation duration of the block. However, unresolvable forks due to non-determinism, are likely a TP problem.

[PREVIOUS_ | FAQ_ | NEXT_]

.. _PREVIOUS: validator.rst
.. _FAQ: README.rst
.. _NEXT: client.rst
.. _Settings: settings.rst

© Copyright 2018, Intel Corporation.
