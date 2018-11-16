Appendix: Sawtooth Settings
==================
[PREVIOUS_ | HOME_ | NEXT_]

.. contents::


This is an unofficial list of some Transaction Family (TF) settings.
There is no central registry, most or all of these can be found on github.

The following are some Sawtooth settings.
Since Sawtooth settings are extensible and include transaction family-specific settings, this list is incomplete.

* You can list existing settings with the
``sawtooth settings list --url http://localhost:8008`` command.
For options, append ``help`` to the command.

* You can set a setting with the ``sawset proposal create --url http://localhost:8008`` command.  For example,
``sawset proposal create --url http://localhost:8008 --key /etc/sawtooth/keys/validator.priv sawtooth.publisher.max_batches_per_block=200``

* Some baseline settings are documented at https://sawtooth.hyperledger.org/docs/core/releases/1.0/transaction_family_specifications/settings_transaction_family.html
* Transactor settings are documented at https://sawtooth.hyperledger.org/docs/core/nightly/master/sysadmin_guide/configuring_permissions.html
  and at https://sawtooth.hyperledger.org/docs/core/nightly/master/transaction_family_specifications/settings_transaction_family.html
* PoET settings are documented at https://sawtooth.hyperledger.org/docs/core/nightly/master/sysadmin_guide/configure_sgx.html
* Validator settings are documented at https://sawtooth.hyperledger.org/docs/core/nightly/master/architecture/injecting_batches_block_validation_rules.html#on-chain-validation-rules
  and https://sawtooth.hyperledger.org/docs/core/nightly/master/architecture/injecting_batches_block_validation_rules.html#on-chain-configuration

sawtooth.config.authorization_type
    Example setting--never used.  To set authorization type, use command line option ``sawtooth-validator --network-auth {trust|challenge}``

sawtooth.consensus.algorithm
    Consensus algorithm (e.g., ``poet`` (PoET SGX or PoET CFT) or ``devmode`` (default) or ``raft`` or any other pluggable consensus engine you provide)
sawtooth.consensus.block_validation_rules
    Lists validation rules to use in deciding what blocks to add to the blockchain.
    See https://sawtooth.hyperledger.org/docs/core/nightly/master/architecture/injecting_batches_block_validation_rules.html
sawtooth.consensus.max_wait_time
    Maximum devmode consensus wait time, in seconds
sawtooth.consensus.min_wait_time
    Minimum devmode consensus wait time, in seconds
sawtooth.consensus.raft.election_tick
    RAFT consensus election tick, in seconds. E.g., 1500
sawtooth.consensus.raft.heartbeat_tick
    RAFT consensus heartbeat tick, in seconds. E.g., 150
sawtooth.consensus.raft.peers
    JSON list of each peer node's public key. Only required RAFT setting.
    Key is from ``/etc/sawtooth/keys/validator.pub`` .
    Example:
    ``["0276f8fed116837eb7646f800e2dad6d13ad707055923e49df08f47a963547b631",\
    "035d8d519a200cdb8085c62d6fb9f2678cf71cbde738101d61c4c8c2e9f2919aa"]``
sawtooth.consensus.raft.period
    RAFT consensus period, in seconds. E.g., 3. Higher settings cause larger blocks, small settings have faster performance with smaller, quicker block publication, but causes more network traffic.
sawtooth.consensus.valid_block_publishers
    List of public keys for allowed block publishers. For devmode

sawtooth.gossip.time_to_live
    Expiration time for the Gossip node communication protocol

sawtooth.identity.allowed_keys
    List of keys allowed to make identity transactions to the identity TP

sawtooth.poet.enclave_module_name
    Python module name implementing the PoET enclave.
    Set to ``sawtooth_poet_sgx.poet_enclave_sgx.poet_enclave``
sawtooth.poet.initial_wait_time
    For C Test: initial time to wait in seconds before proposing a block (e.g., 25; default 3000)
sawtooth.poet.key_block_claim_limit
    For K Test: maximum number of blocks a validator may claim with a PoET keypair before it needs to refresh its signup information (default 250)
sawtooth.poet.population_estimate_sample_size
    Sample size, in blocks, to compute the local mean wait time (default 50).
    The local mean wait time multiplied by random_float(0,1) yields the PoET duration time.
    For production, we recommend 500 to get stable population estimates. Most enterprise networks have stable populations and so a long sample length is preferable. 
sawtooth.poet.report_public_key_pem
    Public key used by Validator Registry TP to verify attestation reports.
    From ``/etc/sawtooth/ias_rk_pub.pem`` or (for PoET CFT) ``/etc/sawtooth/simulator_rk_pub.pem``
sawtooth.poet.target_wait_time
    Target time to wait in seconds before proposing a block (e.g., 5; default 20)
sawtooth.poet.valid_enclave_basenames
    Adds the enclave basename for your enclave to the blockchain for the validator registry transaction processor to use to check signup information.
    From ``poet enclave --enclave-module sgx basename``
sawtooth.poet.valid_enclave_measurements
    Adds the enclave measurement for your enclave to the blockchain for the validator registry transaction processor to use to check signup information.
    From ``poet enclave --enclave-module sgx measurement`` or (for PoET CFT) ``poet enclave measurement``
sawtooth.poet.ztest_minimum_win_count
    For Z Test: minimum win count, to test a node is not winning too frequently

sawtooth.publisher.max_batches_per_block
    Maximum batches allowed per block (e.g., 100)

sawtooth.settings.vote.approval_threshold
    Minimum number of votes required to accept or reject a proposal (default 1)
sawtooth.settings.vote.authorized_keys
    List of public keys for authorized voters for on-chain settings.
    The initial setting is in the Genesis Block, Block 0
sawtooth.settings.vote.proposals
    List of proposals to make changes to settings (base64-encoded ``SettingCandidates`` protobuf)

sawtooth.swa.administrators
    List of public keys for authorized administrators to create, change, or delete Sabre contract and namespace registries.

sawtooth.validator.batch_injectors
    Comma-separated list of batch injectors to load.
    Parsed by validator at beginning of block publishing for each block
sawtooth.validator.block_validation_rules
    On-chain validation rules; enforced by the block validator
sawtooth.validator.max_transactions_per_block
    Maximum transactions allowed per block
sawtooth.validator.transaction_families
    List of permitted transaction families.
    If not set, all transaction families are permitted.
    Example setting:
    ``[{"family":"sawtooth_settings", "version":"1.0"}, {"family":"xo", "version":"1.0"}]``
    *Dan's ProTip*: ``sawtooth_settings`` is a required TF. ``sawtooth_validator_registry`` is required if you use PoET.

transactor
    Public keys of authorized signers (of any kind, batch or transaction)
transactor.batch_signer
    Public keys of authorized batch signers
transactor.transaction_signer
    Public keys of authorized transaction signers
transactor.transaction_signer.<transaction family name>
    Public keys of authorized transaction signers for a transaction processor.
    For a partial list of transaction family names,
    see https://github.com/danintel/sawtooth-faq/blob/master/prefixes.rst
transactor.transaction_signer.intkey
    Public keys of authorized intkey TF signers
transactor.transaction_signer.sawtooth_identity
    Public keys of authorized sawtooth_identity TF signers
transactor.transaction_signer.settings
    Public keys of authorized settings TF signers
transactor.transaction_signer.validator_registry
    Public keys of authorized validator_registry TF signers
transactor.transaction_signer.xo
    Public keys of authorized xo TF signers

[PREVIOUS_ | HOME_ | NEXT_]

.. _PREVIOUS: prefixes.rst
.. _HOME: README.rst
.. _NEXT: permissioning.rst

© Copyright 2018, Intel Corporation.
