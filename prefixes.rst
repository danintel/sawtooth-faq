Appendix: Prefixes
==================
.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

Here is an unofficial list of some Transaction Family (TF) prefixes.
There is no central registry, most or all of these TFs are found on github.
The prefix is either the first 6 characters of the SHA-512 hash of the namespace, or, for some base namespaces, a "hex word".

All data payloads are encoded in base64 after serializing.
Sawtooth headers are serialized with Protobuf.

For base" TF specifications, see
https://sawtooth.hyperledger.org/docs/core/releases/1.0/transaction_family_specifications/

+--------------+----------+--------+------------------------------------------+
|              | SERIAL-  |        |                                          |
| TF NAME      | IZATION  | PREFIX | PREFIX ENCODING                          |
+==============+==========+========+==========================================+
| settings     | Protobuf | 000000 | Validator settings.  Only required TF    |
|              |          |        |                                          |
|              |          |        | 000000<1st 62 chars of                   |
|              |          |        |    SHA-256(policy name)>                 |
+--------------+----------+--------+------------------------------------------+
| identity     | Protobuf | 00001d | Validator Identity for TP/Validator keys |
+--------------+----------+--------+------------------------------------------+
| validator\_   | Protobuf | 6a4372 | Validator Registry,                     |
| registry     |          |        |    to add new validators                 |
+--------------+----------+--------+------------------------------------------+
| blockinfo    | Protobuf | 00b10c | Validator Block Info.  Used for SETH     |
|              |          |        |                                          |
|              |          |        | 00b10c00 metadata namespace              |
|              |          |        |     info about other namespaces          |
|              |          |        |                                          |
|              |          |        | 00b10c01 block info namespace            |
|              |          |        |     historic block info                  |
|              |          |        |                                          |
|              |          |        | 00b10c0100....00<block # in hex>         |
|              |          |        |     info on block at block #             |
+--------------+----------+--------+------------------------------------------+
| sabre        | Protobuf | 00ec00 | WebAssembly VM: NamespaceRegistry        |
|              |          |        |                                          |
|              |          | 00ec01 | Wasm: ContractRegistry                   |
|              |          |        |                                          |
|              |          | 00ec02 | Wasm: Contracts                          |
+--------------+----------+--------+------------------------------------------+
| seth         | Protobuf | a68b06 | SETH (Sawtooth Ethereum VM)              |
+--------------+----------+--------+------------------------------------------+
| rbac         | Protobuf | 8563d0 | T-Mobile NEXT Identity Platform          |
+--------------+----------+--------+------------------------------------------+
|  **SOME EXAMPLE TFs**                                                       |
+--------------+----------+--------+------------------------------------------+
| battleship   | JSON     | 6e10df | Battleship example game                  |
+--------------+----------+--------+------------------------------------------+
| intkey       | CBOR     | 1cf126 | Integer Key. Full production example     |
+--------------+----------+--------+------------------------------------------+
| smallbank    | Protobuf | 332514 | Small Bank example app                   |
+--------------+----------+--------+------------------------------------------+
| xo           | CSV-UTF8 | 5b7349 | Tic-tac-toe example game                 |
+--------------+----------+--------+------------------------------------------+
| supply_chain | Protobuf | 3400de | Asset (Fish) Supply Chain example app    |
+--------------+----------+--------+------------------------------------------+
| simplewallet | CSV-UTF8 | 7e2664 | Simple Wallet minimal example            |
+--------------+----------+--------+------------------------------------------+
| simple\_      | Protobuf | 5d6af4 | Simple Supply example used for training |
| supply       |          |        |                                          |
+--------------+----------+--------+------------------------------------------+
| cookie-maker | raw      | 1a5312 | Cookie Maker example                     |
+--------------+----------+--------+------------------------------------------+

[`PREVIOUS`_]
=========

.. _PREVIOUS: glossary.rst

© Copyright 2018, Intel Corporation.
