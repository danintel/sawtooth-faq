Appendix: Prefixes
==================

Here is an unofficial list of some TF prefixes (there is no central registry):

======================= ======= ===============
TF NAME                 PREFIX  PREFIX ENCODING
======================= ======= ===============
settings                000000  Validator settings
                                000000<1st 62 chars of SHA-256(policy name)>
blockinfo               00b10c  Validator Block Info
                                00b10c00 metadata namespace (info about other namespaces)
                                00b10c01 block info namespace (historic block info)
                                        00b10c0100....00<block # in hex> info on block at block #
identity                00001d  Validator Identity
validator_registry      6a4372  Validator Registry
battleship              6e10df  Battleship game
intkey                  1cf126  Integer key
smallbank               332514  Small Bank app
xo                      5b7349  Tic-tac-toe game
supply_chain            3400de  Asset (Fish) Supply Chain app
simple_supply           5d6af4  Simple Supply example for training class
sabre                   00ec00,00ec01,00ec02 WebAssembly VM
seth                    a68b06  Ethereum VM
rbac                    8563d0  NEXT Identity Platform
cookie-maker            1a5312  Cookie Maker example

© Copyright 2018, Intel Corporation.
