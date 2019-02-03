Sawtooth FAQ: Client
====================

[PREVIOUS_ | FAQ_ | NEXT_]

.. contents::


What is a Sawtooth Client?
--------------------------
It is an application that communicates with the Sawtooth Validator, usually using the REST API. The application is Transaction Family-specific and may come in various forms, such as a CLI, BUI (browser/web app), GUI, or background daemon. The client may be written in any language supported by the Sawtooth SDK.

What languages does the Sawtooth Client SDK support?
----------------------------------------------------
JavaScript, Python 3, and Rust.
Others are in the process of being added: Java and C++.
A SDK for Microsoft .NET is also available from https://github.com/hyperledger/sawtooth-sdk-dotnet
One can also interface directly to Sawtooth without a SDK.
Multiple languages are supported as different languages are more suited for different problem spaces and developers tend to be more comfortable with some languages more than others.
See this chart of Sawtooth SDK support:
https://sawtooth.hyperledger.org/docs/core/releases/latest/app_developers_guide/sdk_table.html
For more information, see the Sawtooth SDK Reference at
https://sawtooth.hyperledger.org/docs/core/releases/latest/sdks.html

Does Sawtooth have a .NET SDK?
------------------------------
Yes, there is a Sawtooth SDK for .NET Core described here:
https://tomislav.tech/2018-03-02-sawtooth-sdk-net-core/
The source is here:
https://github.com/hyperledger/sawtooth-sdk-dotnet

When would you want to develop without the Sawtooth SDK?
--------------------------------------------------------
You should use the SDK whenever possible for your language.
If your preferred development language does not have a SDK,
or if the SDK is incomplete for something you need, then develop without a SDK.
For details, see https://sawtooth.hyperledger.org/docs/core/releases/latest/app_developers_guide/no_sdk.html

What does the client do to send a transaction?
----------------------------------------------
It encodes the TF-specific payload (which could be anything, but defined by the TF) in Base64,
signs the transaction using ECDSA with curve secp256k1, and generates a deterministic state address.

What is the nonce used for in the transaction header?
-----------------------------------------------------
A nonce is a one-time use number (never repeated). Typically a random number is used for a nonce.
A nonce in this case guarantees against replay attacks by making transactions unique.

What does this error mean: ``validator | [... DEBUG signature_verifier] transaction signature invalid for txn: ...``?
-----------------------
The client submitted a transaction with an invalid signature.

What are the various batch_statuses REST API result values?
-----------------------------------------------------------
* ``PENDING`` - batch validation has started on this validator. This ends when the batch is either committed or invalidated
* ``COMMITTED`` - batch is in the blockchain
* ``INVALID`` - batch has recently been invalidated by this validator and is still in the invalid batch cache
* ``UNKNOWN`` - batch is not in any of the above categories, it is not currently being validated by this validator, not in the blockchain, and not in this validator's invalid cache

What does an INVALID batch status mean?
---------------------------------------
I means the transaction batch was processed by the Transaction Processor, but the TP marked it as invalid. The INVALID batch information is not stored on the blockchain. Validators will keep a local cache of invalid batch info around for awhile (I think 10 minutes), so clients can query it, but that data is ephemeral.

What does it mean if a batch status result remains PENDING?
-----------------------------------------------------------
It means processing has not completed on the batch. If it stays that way, it means the transaction batch never reached the Transaction Processor. The transaction remains in the validator queue waiting for the TP to appear online. The TP may have died or may have never started. Or the validator failed the PoET Z Test (z-tested out) because it was winning too frequently.

Can I use partial address prefixes (say the 6-character prefix) in a transaction's input or output list?
--------------------------------------------------------------------------------------------------------
Yes. You can use full addresses or partial addresses or empty (no address). The full addresses are preferred as this allows the parallel scheduler to process non-conflicting transactions in parallel.

How do I debug a Sawtooth client?
---------------------------------
* Add debug messages (such as
  ``print("Action = {}".format(action))`` in Python).
* Start the REST API with the ``sawtooth-rest-api -vvv`` for the most verbosity.
* Set the trace parameter to true when calling method ``Batch``. In Python: ``batch_pb2.Batch(trace=True)`` .
  This prints additional logging information in the Sawtooth REST API and Validator components.

How do I delete or change a specific value in state?
----------------------------------------------------
Use the ``delete_state`` in the SDK to delete a specific state variable.
The data will remain in previously-created blocks (which are immutable),
but will not be in the current blockchain state.

How can a Sawtooth client access a validator on another machine?
----------------------------------------------------------------
By default, the REST API listens to client requests on localhost (127.0.0.1) and is not accessible from a client on another machine. To change this, edit file /etc/sawtooth/rest_api.toml` (copy from `rest_api.toml.example`) and add a line similar to:
``bind = ["10.1.1.2:8008"]`` where you change ``10.1.1.2`` to your IP address or hostname.

How do I use the Rust SDK in MS Windows?
----------------------------------------
`Colin McCullough`_ gives these steps:

1. Download libzmq from https://github.com/zeromq/libzmq and compile from source using ``cmake``. One option is to follow the steps in the ``.appveyor.bat`` file in the ``sawtooth-zmq-0.8.2-dev5`` folder, located in your ``.crates`` folder. It contains appropriate ``cmake`` and ``msbuild`` commands, which may save some time, rather than try to follow the INSTALL instructions that come with libzmq. Windows binaries stop after something around 4.04 and you need 4.2. I believe you also need to rename the built lib file from ``libzmq.lib`` to ``zmq.lib``

2. Download OpenSSL and install. A dependency ``rust-openssl`` states that it will autolocate your ssl installation, but that was not the case for me. You'll have to set the location as an environment variable

3. Download the protobuf compiler, ``protoc``, from https://github.com/protocolbuffers/protobuf/releases and set in your path

4. Set these environment variables ``OPENSSL_DIR``, ``LIBZMQ_INCLUDE_DIR``, ``LIBZMQ_PREFIX``, ``LIBZMQ_LIB_DIR``.  Building may only require the ``OPENSSL_DIR`` and ``LIBZMQ_PREFIX`` to be set.  If you run a build with an incorrect environment variable or you move the dependencies to another location, you must run ``cargo clean`` to reset the build environment.  Rust will look to the old build environment for any environment variables rather than the currently set paths.

5. After all of this I still faced a missing dll error. One of the dependencies needs ``libzmq.dll`` and will not look for it under any of those set variable locations. I put the file in the system32 folder, which fixed the issue.  Any input on what environment variable needs to be set would be helpful.

Until Rust and the RustSDK are a bit more mature, I still recommend using a Docker container for development to avoid any MS Windows troubles.  You can reach me at hello@colinmac.me with any questions.


[PREVIOUS_ | FAQ_ | NEXT_]

.. _PREVIOUS: consensus.rst
.. _FAQ: README.rst
.. _NEXT: rest.rst
.. _Colin McCullough: https://github.com/colincmcc

© Copyright 2018, Intel Corporation.
