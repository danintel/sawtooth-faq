FAQ: Sawtooth Consensus Algorithms (including PoET)
====================

What consensus algorithms does Sawtooth support?
-------------------

dev-mode
    Only suitable for testing TPs with single validator deployments.  Uses a simplified random-leader algorithm for development and testing.  Not for production use
PoET Simulator
    Simulates the SGX environment, and provides CFT similar to Fabric and some other blockchains.  Requires poet-validator-registry TP. Runs on any processor (does not use SGX).  Has Crash Fault Tolerance.
PoET SGX
    Takes advantage of SGX in order to provide BFT like PoW algorithms do, but at very low CPU usage, it is the only algorithm that has hardware requirements (a processor supporting SGX).  Has Byzantine Fault Tolerance

Are there plans to add other consensus algorithms?
-------------------

Yes. We are in the process of adding a fourth consensus, Raft, and may add other consensus engines.

What other consensus algorithms use SGX?
-------------------

Microsoft Azure's Coco consensus algorithm uses SGX.

What is unpluggable consensus?
-------------------
Sawtooth supports unpluggable consensus, meaning you can change the consensus algorithm on the fly,
at a block boundary.
Changing consensus on the fly means it is done without stopping validators, flushing state,
or starting over with a new genesis block.

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

[`PREVIOUS`_] [`NEXT`_]
=========

.. _PREVIOUS: validator.rst
.. _NEXT: client.rst

© Copyright 2018, Intel Corporation.
