Appendix: Sawtooth Transaction Family Prefixes
==================
[PREVIOUS_ | HOME_ | NEXT_]

.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

This is an unofficial list of some Transaction Family (TF) prefixes.
There is no central registry, most or all of these TFs are found on GitHub
( https://github.com/ especially https://github.com/hyperledger and
https://github.com/hyperledger-labs ).

Sawtooth addresses are 70 hex characters.
The prefix is either the first 6 characters of the SHA-512 hash of the namespace, or, for some base namespaces, a "hex word".
The validator_registry uses a SHA-256 hash.
The remainder of the address is TF-specific and defined for each TF.
Listing of a TF does not imply endorsement.

All data payloads are encoded in base64 after serializing.
Sawtooth headers are serialized with Protobuf.

For base TF specifications, see
https://sawtooth.hyperledger.org/docs/core/releases/latest/transaction_family_specifications/

+---------------+-----------+--------+-----------------------------------------+
| TRANSACTION   | SERIAL-   |        |                                         |
| FAMILY NAME   | IZATION   | PREFIX | PREFIX ENCODING                         |
+===============+===========+========+=========================================+
| settings      | Protobuf  | 000000 | Validator settings.  Only required TF   |
+---------------+-----------+--------+-----------------------------------------+
| identity      | Protobuf  | 00001d | Validator Identity for TP/Validator keys|
+---------------+-----------+--------+-----------------------------------------+
| validator\_   | Protobuf  | 6a4372 | PoET Validator Registry. Used by PoET   |
| registry      |           |        | consensus to track other validators     |
+---------------+-----------+--------+-----------------------------------------+
| blockinfo     | Protobuf  | 00b10c | Validator Block Info.  Used for SETH    |
|               |           |        |                                         |
|               |           |        | 00b10c00 metadata namespace             |
|               |           |        | info about other namespaces             |
|               |           |        |                                         |
|               |           |        | 00b10c01 block info namespace           |
|               |           |        | historic block info                     |
|               |           |        |                                         |
|               |           |        | 00b10c0100....00<block # in hex>        |
|               |           |        | info on block at block #                |
+---------------+-----------+--------+-----------------------------------------+
| sabre         | Protobuf  | 00ec00 | WebAssembly VM: NamespaceRegistry       |
|               |           |        |                                         |
|               |           | 00ec01 | Wasm: ContractRegistry                  |
|               |           |        |                                         |
|               |           | 00ec02 | Wasm: Contracts                         |
+---------------+-----------+--------+-----------------------------------------+
| seth          | Protobuf  | a68b06 | SETH (Sawtooth Ethereum VM)             |
+---------------+-----------+--------+-----------------------------------------+
| pdo\_         | Protobuf  | aa2a93 | Private Data Objects (PDO)              |
| contract\_    |           |        | Contract Instance Registry              |
| instance\_    |           |        |                                         |
| registry      |           |        |                                         |
+---------------+-----------+--------+-----------------------------------------+
| pdo\_         | Protobuf  | 0b936f | Private Data Objects (PDO)              |
| contract\_    |           |        | Contract Enclave Registry               |
| enclave\_     |           |        |                                         |
| registry      |           |        |                                         |
+---------------+-----------+--------+-----------------------------------------+
| ccl\_         | Protobuf  | db13a2 | Private Data Objects (PDO)              |
| contract\_    |           |        | Coordination and Commit Log (CCL)       |
| contract\_    |           |        | Contract State Registry                 |
| state\_       |           |        |                                         |
| registry      |           |        |                                         |
+---------------+-----------+--------+-----------------------------------------+
|  **SOME EXAMPLE TFs**                                                        |
+---------------+-----------+--------+-----------------------------------------+
| battleship    | JSON      | 6e10df | Battleship example game                 |
+---------------+-----------+--------+-----------------------------------------+
| intkey        | CBOR      | 1cf126 | Integer Key. Full production example    |
+---------------+-----------+--------+-----------------------------------------+
| smallbank     | Protobuf  | 332514 | Small Bank example app                  |
+---------------+-----------+--------+-----------------------------------------+
| xo            | CSV-UTF8  | 5b7349 | Tic-tac-toe example game                |
+---------------+-----------+--------+-----------------------------------------+
| supply_chain  | Protobuf  | 3400de | Asset (Fish) Supply Chain example app   |
+---------------+-----------+--------+-----------------------------------------+
| marketplace   | Protobuf  | cd6744 | Marketplace example app                 |
+---------------+-----------+--------+-----------------------------------------+
| transfer-chain| JSON-UTF8 | 19d832 | Simple Tuna Supply Chain app.           |
|               |           |        | Used for edX LFS171x class              |
+---------------+-----------+--------+-----------------------------------------+
| simplewallet  | CSV-UTF8  | 7e2664 | Simple Wallet minimal example           |
+---------------+-----------+--------+-----------------------------------------+
| cookiejar     | CSV-UTF8  | a4d219 | Cookie Jar minimal example              |
+---------------+-----------+--------+-----------------------------------------+
| simple\_      | Protobuf  | 5d6af4 | Simple Supply example used for future   |
| supply        |           |        | edX LFS201 class                        |
+---------------+-----------+--------+-----------------------------------------+
| pirate-talk   | UTF8      | aaaaaa | Pirate Talk minimal example             |
+---------------+-----------+--------+-----------------------------------------+
| cookie-maker  | raw       | 1a5312 | Cookie Maker minimal example            |
+---------------+-----------+--------+-----------------------------------------+
|  **SOME THIRD-PARTY PRODUCTION TFs**                                                        |
+---------------+-----------+--------+-----------------------------------------+
| rbac          | Protobuf  | 8563d0 | T-Mobile NEXT Identity Platform         |
+---------------+-----------+--------+-----------------------------------------+
| pub_key       | Protobuf  | a23be1 | REMME REMChain                          |
+---------------+-----------+--------+-----------------------------------------+
| bitagora\-    | Protobuf  | b42861 | Bitagora voting ballot                  |
| ballots       |           |        |                                         |
+---------------+-----------+--------+-----------------------------------------+
| bitagora\-    | Protobuf  | 154f9c | Bitagora voting polls                   |
| polls         |           |        |                                         |
+---------------+-----------+--------+-----------------------------------------+

[PREVIOUS_ | HOME_ | NEXT_]

.. _PREVIOUS: glossary.rst
.. _HOME: README.rst
.. _NEXT: settings.rst

© Copyright 2018, Intel Corporation.
