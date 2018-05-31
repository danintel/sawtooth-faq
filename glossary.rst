Appendix: Glossary
==================
.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

See also:

* https://sawtooth.hyperledger.org/docs/core/nightly/master/glossary.html
* https://sawtooth.hyperledger.org/docs/core/releases/1.0/architecture/poet.html#definitions


Address (aka State Address)
    For Sawtooth, each radix address (or Node ID) into a Merkle Trie is 70 hex characters (35 bytes). The first 6 characters, the prefix, encode the name space (of the TF) and the remaining bytes are implementation-dependent.  The prefix is either the first 6 characters of the SHA-512 hash of the namespace, or a hex word for base name spaces.  See the list of TF prefixes in the Appendix
AVR
    Attestation Verification Report. Response body signature, signed with the IAS Report Key
back pressure
    flow-control technique to help prevent Denial of Service attacks; used by Sawtooth
Batch
    A set of transactions that must be made together (atomic commit) to maintain state consistency
Block
    A set of records of permanent transactions; these blocks are linked into a blockchain.  A block is similar to a page in a ledger book, where the ledger book is a blockchain
Blockchain
    A single-link list of blocks.  The blockchain is immutable, distributed, and cryptographically-secured
Block ID
    128 hex character ID (64 bytes) identifying a block in a blockchain
BFT
    Byzantine Fault Tolerance.  Consensus is possible with malicious actors (Byzantine Generals Problem). BFT is stronger than CFT and uses voting
C
    signup delay; number of blocks a validator has to wait before participating in elections (when using PoET)
C Test
    test that a block-claiming validator follows C, signup delay
CFT
    Crash Fault Tolerance. Consensus possible even with failed components
Client
    Any program that creates a transaction; interfaces with the validator using REST.  Does not have to be a web-based app
Consensus algorithm
    Method to decide what block to add next to a blockchain
Classical Consensus
    Uses an agreement or voting mechanism (vs. Nakamoto-style consensus)
Docker
    A light-weight OS-level VM technology which isolates processes into separate "containers"
Duplicity
	A faulty node sending deceitful or inconsistent messages to other nodes
CSV
    Comma separated values.  E.g.: ``a,b,c,d``
Curve325519
    An ECDH (Elliptic Curve Diffie-Hellman) key agreement protocol used by Sawtooth. Used by validators in ZMQ connections to exchange keys
Data model
    Can be any format (CSV, protobufs, etc.)
Deterministic
    Means consistent, or the same. For Sawtooth, serialization must be deterministic, meaning the encoding is always in the same order and always the same for the same data.  Many JSON libraries do not encode data deterministically.
EPID
    Enhanced Privacy ID. An anonymous credential system; used by PoET
Enclave
    SGX-protected area of data and code to provide confidentiality and integrity even against privileged malware
EVM
    Ethereum Virtual Machine. Executes machine-independent code for Ethereum.  Supported by SETH on Sawtooth
Fork
    When network nodes have two competing nodes at the head of a blockchain
Gossip
    A message broadcast mechanism that uses forwarding to random peers (Sawtooth Validator nodes)
Hyperledger
    A family of distributed ledgers sponsored by The Linux Foundation, including Sawtooth
IAS
    Intel Attestation Server. Used to authenticate PoET SGX keys; runs in public Internet at https://as.sgx.trustedservices.intel.com/
IntKey
    Integer key TP. Sample Sawtooth TP that implements set/increment/decrement/show operations
K
    Claim limit, number of blocks a validator can publish before it must signup again (when using PoET)
K Test
    Test a block-claiming validator follows K, claim limit before another signup
Liveness
    A consensus algorithm property where the nodes eventually must agree on a value
Marshalling
    serialization of data
Merkle Tree (or Trie)
    a radix search tree data structure with addressable nodes. Used to store state
n
    Nodes in a blockchain network
Nakamoto-style Consensus
    uses some sort of lottery-based mechanism, such as Proof of Work (vs. Classical Consensus)
Node ID
    Address
Node
    See Validator
Nonce
    A one-time number; usually random, but must not predictably repeat (such as after reboot/restart)
One-say, all-adopt
	Strategy where only a single multicast round of messages reaches agreement
Payload
    Data processed by the TP and only the TP. Can be any format (CSV, protobufs, etc.) Data model is defined by TF. Payload is encoded using MIME's Base64 (``A-Za-z0-9+/``) + ``=`` for 0 mod 4 padding
PBFT
    Practical Byzantine Fault Tolerance. A "classical" consensus algorithm that uses a state machine. PBFT is a three-phase, network-intense algorithm, so is not scalable to large networks
Permissioned Blockchain (aka Private Blockchain)
    participants must ID themselves to a network (e.g., Hyperledger Sawtooth or Hyperledger Fabric)
Permissionless Blockchain (aka Public Blockchain)
    anyone can join network (e.g., Bitcoin, Ethereum)
PoET
    Proof of Elapsed Time (optional Nakamoto-style consensus algorithm used for Sawtooth). PoET with SGX has BFT. PoET Simulator has CFT. Not CPU-intensitve as with PoW-style algorithms, although it still can fork and have stale blocks.  See PoET specification at https://sawtooth.hyperledger.org/docs/core/releases/latest/architecture/poet.html
PoW
    Proof of Work. Completing work (CPU-intensive Nakamoto-style consensus algorithm). Usually used in permissionless blockchains
PoS
    Proof of Stake. Nakamoto-style consensus algorithm based on the most wealth or age (stake)
Private Blockchain
    See Permissioned Blockchain
Proposal
    proposed block from a validator to add to a blockchain
Protobuf
    Serialization/data interchange library used by Sawtooth
PDO
    Private Data Object. Blockchain objects that are kept private through encryption
Public Blockchain
    See Permissionless Blockchain
r
    Rate, measurement of performance in transactions per second
Raft
    Consensus algorithm that elects a leader for a term of arbitrary time.  Raft is CFT, but not BFT
Replica
    Another term for node or validator
REST
     Representational State Transfer. Industry-standard web-based API.  REST is available on a Sawtooth validator node through TCP port 8008.  For more information, see the Sawtooth REST API Reference at https://sawtooth.hyperledger.org/docs/core/releases/latest/rest_api.html
Sabre
    TF that implements on-chain smart contracts with the WebAssembly VM.  For more information, see Sabre RFC at https://github.com/hyperledger/sawtooth-rfcs/blob/master/text/0007-wasm-smart-contracts.md
Stale block
     A block proposed to be at the head of a blockchain, but lost to a competing block that became the head as decided by the consensus algorithm
TF
    Transaction Family. Consists of the Client, State, and TP
TP
    Transaction Processor. Processes transactions for a specific TF.  Runs on Validator. Similar to a Ethereum "smart contract" or Bitcoin "chain code"
Safety
    A consensus algorithm property where the "honest" (non-Byzantine) nodes agree on the same value
Sawtooth
    Permissioned blockchain platform for running distributed ledgers
SETH
     Ethereum-compatible Sawtooth Transaction Processor. Suppors running Ethereum Virtual Machine
secp256k1
    An ECDSA (Elliptic Curve DSA) cryptographic algorithm used by Sawtooth with a 32-byte key. Used for Validator and TP. Bitcoin also uses this algorithm
Serialization
    A scheme to encode data as a byte stream.  For Sawtooth the serialization must be deterministic, meaning the encoding is always in the same order and always the same for the same data.  Protobufs are often used in Sawtooth Serialization, but that is not a requirement.  A simpler alternative, for example, is CSV.
SGX
    Intel Software Guard Extensions. Specialized hardware that provides enclaves with protected code and data. Used to implement PoET SGX
State
    The current information for each Transaction Family.  The global state is stored in a Merkle Tree. View local validator through http://localhost:8008/state
State Address
    See Address
Sybil Attacks
    Using forged identities in a blockchain network to subvert the reputation system. Was named after the book and movie
Validator
    Validates transactions and sends to the appropriate TP; proposes new blocks for block chain
Validator
    Validates transactions and sends to the appropriate TP; proposes new blocks for block chain usually in a network of validator nodes
VM
    Virtual Machine
Wasm
    See WebAssembly
WebAssembly
    A stack-based VM newly-implemented in major web browsers. It is well-suited for the purposes of smart contract execution due to its sandboxed design, growing popularity, and tool support. Sabre implements WebAssembly
XO
    Example Sawtooth TP that implements the Tic-tac-toe game
Z Test
    Test a block-claiming validator is not winning too frequently
ZMQ (aka 0MQ, ZeroMQ)
    Message Transport API available on Linux; used by Sawtooth Validator nodes
ZKP
    Zero Knowledge Proof. One party proving they know a value x without conveying x

[`PREVIOUS`_] [`HOME`_] [`NEXT`_]
=========

.. _PREVIOUS: docker.rst
.. _HOME: README.md
.. _NEXT: prefixes.rst

© Copyright 2018, Intel Corporation.
