Unofficial FAQ: Sawtooth Client
====================
[PREVIOUS_ | HOME_ | NEXT_]

.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

What is a Sawtooth Client?
--------------------------
It is an application that communicates with the Sawtooth Validator, usually using the REST API.  The application is Tranaction Family-specific and may come in various forms, such as a CLI, BUI (browser/web app), GUI, or background daemon.  The client may be written in any language supported by the Sawtooth SDK.

What languages does the Sawtooth SDK support?
-------------------
Python 3, Go, Javascript, Rust, Java, and C++.  The "best" supported SDKs are Python 3 and Rust.
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

[PREVIOUS_ | HOME_ | NEXT_]

.. _PREVIOUS: consensus.rst
.. _HOME: README.rst
.. _NEXT: rest.rst

© Copyright 2018, Intel Corporation.
