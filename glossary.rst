
See also:
* https://sawtooth.hyperledger.org/docs/core/nightly/master/glossary.html
* https://sawtooth.hyperledger.org/docs/core/releases/1.0/architecture/poet.html#definitions


Address (aka State Address) - For Sawtooth, each radix address (or Node ID) into a Merkle Trie is 70 hex characters (35
bytes). The first 6 characters, the prefix, encode the name space (of the TF) and the remaining bytes are implementation
-dependent.
The prefix is usually the first 6 characters of the SHA-512 hash of the namespace, or a hex word for base name spaces.
See the list of TF prefixes in the Appendix

AVR - Attestation Verification Report - Response body signature, signed with the IAS Report Key
back pressure - flow-control technique to help prevent Denial of Service attacks; used by Sawtoot
Batch - A set of transactions that must be made together (atomic commit) to maintain state consistency
Block - A set of records of permanent transactions; these blocks are linked into a blockchain.
        A block is similar to a page in a ledger book, where the ledger book is a blockchain
Blockchain - a single-link list of blocks.  The blockchain is immutable, distributed, and cryptographically secured
Block ID - 128 hex character ID (64 bytes) identifying a block in a blockchain
BFT - Byzantine Fault Tolerance - consensus possible with malicious actors (Byzantine Generals Problem);
        stronger than CFT; uses voting
C - signup delay; number of blocks a validator has to wait before participating in elections (when using PoET)
C Test - test that a block-claiming validator follows C, signup delay
CFT - Crash Fault Tolerance - consensus possible even with failed components
Client - any program that creates a transaction; interfaces with the validator using REST.
        Does not have to be a web-based app
Consensus algorithm - method to decide what block to add next to a blockchain
Classical Consensus - uses an agreement or voting mechanism (vs. Nakamoto-style consensus)
Docker - a light-weight OS-level VM technology which isolates processes into separate "containers"
PoET - Proof of Elapsed Time (optional Nakamoto-style consensus algorithm used for Sawtooth)
CSV - comma separated values.  E.g.: a,b,c,d
Curve325519 - An ECDH (Elliptic Curve Diffie-Hellman) key agreement protocol used by Sawtooth
        - Used by validators in ZMQ connections to exchange keys
Data model - Can be any format (CSV, protobufs, etc.)
Deterministic - means consistent, or the same. For Sawtooth, serialization must be deterministic,
        meaning the encoding is always in the same order and always the same for the same data.
        Many JSON libraries do not encode data deterministically.
EPID - Enhanced Privacy ID - an anonymous credential system; used by PoET
Enclave - SGX protected area of data and code to provide confidentiality and integrity even against privileged malware
EVM - Ethereum Virtual Machine  - executes machine-independent code for Ethereum.  Supported by SETH on Sawtooth
Gossip - A message broadcast mechanism that uses forwarding to random peers (Sawtooth Validator nodes)
Hyperledger - A family of distributed ledgers sponsored by The Linux Foundation, including Sawtooth
IAS - Intel Attestation Server - used to authenticate PoET SGX keys; runs in public Internet
        - At https://as.sgx.trustedservices.intel.com/
IntKey - Integer key TP - sample Sawtooth TP that implements set/increment/decrement/show operations
K - Claim limit, number of blocks a validator can publish before it must signup again (when using PoET)
