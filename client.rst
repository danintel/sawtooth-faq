Preliminary FAQ: Sawtooth Client
====================
[`PREVIOUS`_ | `HOME`_ | `NEXT`_]
---------------------------------
.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

What languages does the Sawtooth SDK support?
-------------------

Python 3, Go, Javascript, and Rust.  The "best" supported SDKs are Python 3 and Rust.
One can also interface directly to Sawtooth without a SDK.
For more information, see the Sawtooth SDK Reference at
https://sawtooth.hyperledger.org/docs/core/releases/latest/sdks.html

What does the client do to send a transaction?
-------------------
It encodes the TF-specific payload (which could be anything, but defined by the TF) in Base64,
signs the transaction using ECDSA with curve secp256k1, and generates a deterministic state address.

What is the nonce used for in the transaction header?
-------------------
A nonce is a one-time use number (never repeated).  Typically a random number is used for a nonce.
A nonce in this case guarantees against replay attacks by making transactions unique.

[`PREVIOUS`_ | `HOME`_ | `NEXT`_]
---------------------------------

.. _PREVIOUS: consensus.rst
.. _HOME: README.md
.. _NEXT: rest.rst

© Copyright 2018, Intel Corporation.
