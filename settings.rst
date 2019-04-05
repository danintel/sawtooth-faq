Appendix: Sawtooth Settings
===========================

[PREVIOUS_ | FAQ_ | NEXT_]

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

* Some baseline settings are documented at https://sawtooth.hyperledger.org/docs/core/releases/latest/transaction_family_specifications/settings_transaction_family.html
* Transactor settings are documented at https://sawtooth.hyperledger.org/docs/core/nightly/master/sysadmin_guide/configuring_permissions.html
  and at https://sawtooth.hyperledger.org/docs/core/nightly/master/transaction_family_specifications/settings_transaction_family.html
* PoET settings are documented at https://sawtooth.hyperledger.org/docs/core/nightly/master/sysadmin_guide/configure_sgx.html
* Validator settings are documented at https://sawtooth.hyperledger.org/docs/core/nightly/master/architecture/injecting_batches_block_validation_rules.html#on-chain-validation-rules
  and https://sawtooth.hyperledger.org/docs/core/nightly/master/architecture/injecting_batches_block_validation_rules.html#on-chain-configuration

sawtooth.config.authorization_type
    Example setting--never used.  To set authorization type, use command line option ``sawtooth-validator --network-auth {trust|challenge}``

sawtooth.consensus.algorithm.name
    Pluggable consensus algorithm name. These include ``PoET``, ``Devmode``, ``sawtooth-raft-engine``, and ``sawtooth-pbft``.  The default is ``devmode`` for Sawtooth 1.1 or earlier, with no default for the (unreleased) nightly build.
sawtooth.consensus.algorithm.version
    Consensus algorithm version. Currently 0.1 for PoET, Devmode, and sawtooth-pbft, and 0.1.0 for sawtooth-raft-engine.
sawtooth.consensus.block_validation_rules
    Lists validation rules to use in deciding what blocks to add to the blockchain.
    See https://sawtooth.hyperledger.org/docs/core/nightly/master/architecture/injecting_batches_block_validation_rules.html
sawtooth.consensus.max_wait_time
    Maximum devmode consensus wait time, in seconds
sawtooth.consensus.min_wait_time
    Minimum devmode consensus wait time, in seconds

sawtooth.consensus.pbft.members
    JSON list of public keys belonging to each node that is a member of the PBFT
    network. Only required PBFT setting.
    Key is from ``/etc/sawtooth/keys/validator.pub`` .
    Example:
    ``["0276f8fed116837eb7646f800e2dad6d13ad707055923e49df08f47a963547b631",\
    "035d8d519a200cdb8085c62d6fb9f2678cf71cbde738101d61c4c8c2e9f2919aa"]``

    For details on this and other PBFT settings, see
    https://github.com/hyperledger/sawtooth-pbft/blob/master/src/config.rs
sawtooth.consensus.pbft.block_duration
    How often to try to publish a block. Optional, default 200 ms.
sawtooth.consensus.pbft.exponential_retry_base
    Base time to use for retrying with exponential backoff
    Optional, default 100 ms
sawtooth.consensus.pbft.exponential_retry_max
    Maximum time for retrying with exponential backoff.
    Optional, default 60s
sawtooth.consensus.pbft.idle_timeout
    How long to wait for the next (BlockNew + PrePrepare) before determining
    primary is faulty. Must be longer than block_duration.
    Optional, default 30s
sawtooth.consensus.pbft.commit_timeout
    How long to wait (after Pre-Preparing) for the node to commit the block
    before starting a view change
    Optional, default 30s
sawtooth.consensus.pbft.view_change_duration
    If a node on view v starts a change to view (v + n), the timeout before
    starting a new change to view (v + n + 1) will be (n * view_change_duration)
    Optional, default 5s
sawtooth.consensus.pbft.forced_view_change_period
    How many blocks to commit before forcing a view change for fairness
    Optional, default 30 blocks
sawtooth.consensus.pbft.message_timeout
    How long to wait for updates from the Consensus API.
    Optional, default 10 ms.
sawtooth.consensus.pbft.max_log_size
    How large the PBFT message log is allowed to get.
    Optional, default 1000 messages

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
    For C Test: initial time to wait in seconds before proposing a block (e.g., 25; default is 3000)
sawtooth.poet.block_claim_delay
    For C Test: block claim delay in blocks.
    Set to 1 to prevent most reasonable attacks.
    Set to 2 or 3 if you want more aggressive protection. Default is 1.
sawtooth.poet.key_block_claim_limit
    For K Test: maximum number of blocks a validator may claim with a PoET keypair before it needs to refresh its signup information.
    I recommend bumping up so each key is good for 100000 blocks.
    A big number reduces the likelihood that validator keys will expire simultaneously and deadlock the network. Default is 250
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
    For Z Test: minimum win count, to test a node is not winning too frequently.
    For test networks, disable by setting to 999999999, which gives you several decades before the Z test kicks in (16 years * 5 validators @ 30 seconds/block). This test is meant to catch rogue validators who have broken their enclave and are publishing too frequently.  The Z Test doesn't work on small networks because all validators publish often

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


[PREVIOUS_ | FAQ_ | NEXT_]

.. _PREVIOUS: prefixes.rst
.. _FAQ: README.rst
.. _NEXT: permissioning.rst

© Copyright 2018, Intel Corporation.
