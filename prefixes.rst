Appendix: Prefixes
==================
[`PREVIOUS`_]
-------------

Here is an unofficial list of some Transaction Family (TF) prefixes
(there is no central registry, most or all are on github):

::

    TF NAME                 PREFIX  PREFIX ENCODING
    settings                000000  Validator settings.  Only required TF
                                    000000<1st 62 chars of SHA-256(policy name)>
                                    000000<1st 62 chars of SHA-256(policy name)>
    blockinfo               00b10c  Validator Block Info.  Used for SETH
                                    00b10c00 metadata namespace
                                                (info about other namespaces)
                                    00b10c01 block info namespace
                                                (historic block info)
                                            00b10c0100....00<block # in hex>
                                                info on block at block #
    identity                00001d  Validator Identity
    validator_registry      6a4372  Validator Registry
    sabre                   00ec00,00ec01,00ec02 WebAssembly VM
    seth                    a68b06  SETH (Sawtooth Ethereum VM)
    rbac                    8563d0  NEXT Identity Platform

    SOME EXAMPLE TFs
    battleship              6e10df  Battleship example game
    intkey                  1cf126  Integer key. Full production example
    smallbank               332514  Small Bank example app
    xo                      5b7349  Tic-tac-toe example game
    supply_chain            3400de  Asset (Fish) Supply Chain example app
    simplewallet            7e2664  Simple wallet minimal example
    simple_supply           5d6af4  Simple Supply example used for training
    cookie-maker            1a5312  Cookie Maker example

[`PREVIOUS`_]
=========

.. _PREVIOUS: glossary.rst

© Copyright 2018, Intel Corporation.
