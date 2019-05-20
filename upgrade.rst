---
layout: page
hide: true
tags: [faq]
title: Sawtooth FAQ - Upgrading Hyperledger Sawtooth
permalink: /faq/upgrade/
# Copyright (c) 2019, Intel Corporation.
# Licensed under Creative Commons Attribution 4.0 International License
# https://creativecommons.org/licenses/by/4.0/
---
Sawtooth FAQ: Upgrading Hyperledger Sawtooth
============================================

.. class:: mininav

PREVIOUS_ FAQ_ NEXT_

.. contents::


Points to consider while upgrading Hyperledger Sawtooth
-------------------------------------------------------
This is applicable to upgrade from version 1.0.X to 1.1.X

1. Persist the block store, transaction receipt and global state (lmdb files
   default stored in /var/lib/sawtooth directory).
2. Persist the keys generated for the validator (default stored in
   /etc/sawtooth directory).
3. Hyperledger Sawtooth 1.1.X versions have a major feature introduced where
   different consensus engine can be plugged into the validator.

   - Validator shall open the listener port so that the consensus engine can
     be registered to it. This in addition to other ports which are open to
     transaction processor/event listeners/clients, validator connections.
     For example, command line option to listen on localhost is as follows

.. code:: sh

    sawtooth-validator --bind consensus:tcp://localhost:5050

4. When a validator service is restarted, you need to re-register the
   transaction processor, consensus engine or other components such as event
   listeners. This is true for all the components which initiates a connection
   to the validator service, except for the case of validator itself initiates
   a connection.
5. Submitted transactions are stored in memory, these are lost when the service
   is brought down or a new service is brought up.
6. Client should have a retry mechanism for all the transactions until they are
   either considered in a block or rejected by the validator.

**Note:** The validator service would be unavailable for the time period when
the upgrade happens. Client can consider this as validator service unavailable
and retry the transactions when the validator service is up again.

Transaction Processor upgrades
------------------------------
- Maintain old versions of transaction processors, even though clients are not
  sending transactions to the old version. This is required so that when a new
  validator is added to the network and it needs to catch up with earlier
  published blocks.

Consensus engine upgrades
-------------------------
- Maintain older version of consensus engines for a newly added node in the
  network, to catch up with the earlier blocks.

General practice for deploying a Hyperledger Sawtooth application
-----------------------------------------------------------------
- Transactions may be rejected by a validator if the input rate is higher than
  what a validator can handle in its queue. The queue maintained by the
  validator does not have a fixed size, rather the size depends on the rate at
  which blocks are published. Client should consider this queue full case at
  validator end and retry the same transaction when the validator is able to
  handle.

.. class:: mininav

PREVIOUS_ FAQ_ NEXT_

.. _PREVIOUS: /faq/docker/
.. _FAQ: /faq/
.. _NEXT: /faq/glossary/

© Copyright 2018, Intel Corporation.
