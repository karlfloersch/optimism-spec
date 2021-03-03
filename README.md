# Launch scope

## Goals
* Add an RPC to metamask (with wallet_addEthereumChain hopefully, or manually)
* Switch the compiler from solc -> solcovm
* Optimism L2 should behave like L1
* Native currency on both is ETH, and fees are paid similarly
* Fast deposit by sending ETH to a special address on L1, shows up in L2
* Slow withdrawal by sending ETH to a special address on L2, shows up back in L1 7 days later
* Generic trusted messaging bridge people can use as a primitive (TODO: add link here to API)

## Non Goals
* For now, a fully controlled Proof of Authority chain is okay
* Single trusted sequencer, no fraud proofs
* Contracts must be compiled with OVM, and hence must be smaller since the OVM adds some overhead, and deployment uses a lot of gas (due to safety checking?)
* Value does not work in contracts (wait huh? I don't think this is okay. uniswap relay won't work)

# Implementation

## Flow of a tx
* tx is submitted into [l2geth](https://github.com/ethereum-optimism/go-ethereum) (TODO: rename this repo to l2geth?)
* It is processed mostly by the [contracts](https://github.com/ethereum-optimism/contracts) that define the OVM operating system (TODO: rename this ovm-os-contracts or something)
* The single [batch submitter](https://github.com/ethereum-optimism/batch-submitter) submits both the transaction and the new state root
* The [data transport layer](https://github.com/ethereum-optimism/data-transport-layer) (TODO: rename this repo to l1-indexer or something) reads the submitted transaction and state root in L1.

## Number of lines
* Compiler -- 300?
* l2geth -- 1000?
** This should fetch the addresses from address manager through data-transport-layer
** This should do very little processing, and leave that to the Operating System Contracts
* data-transport-layer -- 2243 (this should merge into l2geth)
* batch-submitter -- 1243
* optimism-ts-services -- 2312 (this should merge with batch-submitter)
** Message passer
** Fraud prover submission
* Contracts -- 8414!
** Operating System Contracts (L2 contracts)
** Fraud prover contracts (L1 contracts)

## Upgradablity
* Contracts on L1 can be upgraded?
* The L2 contracts can be upgraded using transactions from L1
* L2 OS contracts come from a special genesis contract on L1 (remove state-dump.latest.json)

