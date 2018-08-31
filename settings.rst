Appendix: Sawtooth Settings
==================
[PREVIOUS_ | HOME_]

.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

This is an unofficial list of some Transaction Family (TF) settings.
There is no central registry, most or all of these can be found found on github.

The following are some Sawtooth settings.
Since Sawtooth settings are extensible and include transaction family-specific settings, this list is incomplete.

* Some baseline settings are documented at https://sawtooth.hyperledger.org/docs/core/releases/1.0/transaction_family_specifications/settings_transaction_family.html
* Transactor settings are documented at https://sawtooth.hyperledger.org/docs/core/nightly/master/sysadmin_guide/configuring_permissions.html
and at
https://sawtooth.hyperledger.org/docs/core/nightly/master/transaction_family_specifications/settings_transaction_family.html
* PoET settings are documented at https://sawtooth.hyperledger.org/docs/core/nightly/master/sysadmin_guide/configure_sgx.html

sawtooth.config.authorization_type
    Example setting--never used

sawtooth.consensus.algorithm
    Consensus algorith (e.g., ``poet``)
sawtooth.consensus.block_validation_rules
sawtooth.consensus.max_wait_time
sawtooth.consensus.min_wait_time
sawtooth.consensus.valid_block_publishers

sawtooth.gossip.time_to_live

sawtooth.identity.allowed_keys
    List of nodes allowed to connect to the Sawtooth network

sawtooth.poet.enclave_module_name
	Python module name implementing the PoET enclave.  Set to ``sawtooth_poet_sgx.poet_enclave_sgx.poet_enclave -o config.batch``
sawtooth.poet.initial_wait_time
sawtooth.poet.key_block_claim_limit
sawtooth.poet.population_estimate_sample_size
sawtooth.poet.report_public_key_pem
    Public key used by Validator Registry TP to verify attestation reports. From ``/etc/sawtooth/ias_rk_pub.pem``
sawtooth.poet.target_wait_time
sawtooth.poet.valid_enclave_measurements
    Adds the enclave measurement for your enclave to the blockchain for the validator registry transaction processor to use to check signup information. From ``poet enclave --enclave-module sgx measurement``
sawtooth.poet.valid_enclave_basenames
	Adds the enclave basename for your enclave to the blockchain for the validator registry transaction processor to use to check signup information. From ``poet enclave --enclave-module sgx basename``
sawtooth.poet.ztest_minimum_win_count

sawtooth.publisher.max_batches_per_block

sawtooth.settings.vote.authorized_keys
    List of public keys for authorized voters for on-chain settings. The initial setting is in the Genesis Block, Block 0
sawtooth.settings.vote.proposals
    List of proposals to make changes to settings (``SettingCandidates`` protobuf base64-encoded)
sawtooth.settings.vote.approval_threashold
    Minimum number of votes required to accept or reject a proposal (default 1)

sawtooth.validator.batch_injectors
sawtooth.validator.block_validation_rules
sawtooth.validator.transaction_families List of permitted transaction families
sawtooth.validator.max_transactions_per_block

transactor.batch_signer Public keys of authorized batch signers

transactor.transaction_signer Public keys of authorized transaction signers
transactor.transaction_signer.<transaction family name> Public keys of authorized transaction signers for a transaction processor.  For a partial list of transaction family names, see https://github.com/danintel/sawtooth-faq/blob/master/prefixes.rst 
transactor.transaction_signer.intkey
transactor.transaction_signer.sawtooth_identity
transactor.transaction_signer.settings
transactor.transaction_signer.validator_registry
transactor.transaction_signer.xo

[PREVIOUS_ | HOME_]

.. _PREVIOUS: prefixes.rst
.. _HOME: README.rst

© Copyright 2018, Intel Corporation.
