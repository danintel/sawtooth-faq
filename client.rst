Unofficial FAQ: Sawtooth Client
====================
[PREVIOUS_ | HOME_ | NEXT_]

.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

What is a Sawtooth Client?
--------------------------
It is an application that communicates with the Sawtooth Validator, usually using the REST API.  The application is Transaction Family-specific and may come in various forms, such as a CLI, BUI (browser/web app), GUI, or background daemon.  The client may be written in any language supported by the Sawtooth SDK.

What languages does the Sawtooth Client SDK support?
-------------------
JavaScript, Python 3, and Rust.
Others are in the process of being added: Java and C++.
One can also interface directly to Sawtooth without a SDK.
Multiple languages are supported as different languages are more suited for different problem spaces and developers tend to be more comfortable with some languages more than others.
See this chart of Sawtooth SDK support:
https://sawtooth.hyperledger.org/docs/core/releases/latest/app_developers_guide/sdk_table.html
For more information, see the Sawtooth SDK Reference at
https://sawtooth.hyperledger.org/docs/core/releases/latest/sdks.html

When would you want to develop without the Sawtooth SDK?
------------------------------------------------------
You should use the SDK whenever possible for your language.
If your preferred development language does not have a SDK,
or if the SDK is incomplete for something you need, then develop without a SDK.
For details, see https://sawtooth.hyperledger.org/docs/core/releases/latest/app_developers_guide/no_sdk.html

What does the client do to send a transaction?
-------------------
It encodes the TF-specific payload (which could be anything, but defined by the TF) in Base64,
signs the transaction using ECDSA with curve secp256k1, and generates a deterministic state address.

What is the nonce used for in the transaction header?
-------------------
A nonce is a one-time use number (never repeated).  Typically a random number is used for a nonce.
A nonce in this case guarantees against replay attacks by making transactions unique.

What does this error mean: ``validator | [... DEBUG signature_verifier] transaction signature invalid for txn: ...``?
-----------------------
The client submitted a transaction with an invalid signature.

What does an INVALID batch status mean?
-------------------------
I means the transaction batch was processed by the Transaction Processor, but the TP marked it as invalid.

What does a PENDING batch status mean?
--------------------------
It means the transaction batch never reached the Transaction Processor.  The TP may have died or have never started. Or the validator failed the Z Test (z-tested out) because it was winning too frequently.

Can I use partial address prefixes (say the 6-character prefix) in a transaction's input or output list?
------------------------
Yes.  You can use full addresses or partial addresses or empty (no address).  The full addresses are preferred as this allows the parallel scheduler to process non-conflicting transactions in parallel.

How do I debug a Sawtooth client?
---------------------------
* Add debug messages (such as
``print("Action = {}".format(action))`` in Python).
* Start the REST API with the ```sawtooth-rest-api -vvv`` for the most verbosity.
* Set the trace parameter to true when calling method ``Batch``. In Python: ``batch_pb2.Batch(trace=True)`` .
This prints additional logging information in the Sawtooth REST API and Validator components.



[PREVIOUS_ | HOME_ | NEXT_]

.. _PREVIOUS: consensus.rst
.. _HOME: README.rst
.. _NEXT: rest.rst

© Copyright 2018, Intel Corporation.
