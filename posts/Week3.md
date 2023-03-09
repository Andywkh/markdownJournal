# Week 3 [16012023 - 20012023:]

## ================== What is Cosmos ? ==================

Cosmos is a decentralized network of independent parallel blockchains, each powered by a Byzantine Fault-Tolerance consensus algorithm. A Byzantine Fault-Tolerant consensus algorithm guarantees safety for up to a third of Byzantine, or malicious, actors.

Before Cosmos, blockchains were siloed and unable to communicate with each other. They were hard to build and could only handle a small amount of transactions per second. The vison of Cosmos is to create an Internet of Blockchains, a network of blockchains able to communicate with each other in a decentralized way, achieved through a set of open source tools like Tendermint, the Cosmos SDK and IBC designed to let people build custom, secure, scalable and interoperable blockchain applications quickly.

<ul>
	<li>Tendermint is both a consensus engine and a BFT consensus algorithm. State-machines can be built in any programming language on top of Tendermint, which takes care of replication.</li>
	<li>The Cosmos SDK is a modular framework that simplifies the process of building secure blockchain applications.</li>
	<li>IBC is the Inter-Blockchain Communication protocol that can be thought of as the TCP/IP for blockchains. IBC allows fast-finality blockchains to exchange value and data with each other in a decentralized way.</li>
</ul>

Recap on Cosmos:

<ul>
	<li>Cosmos makes blockchains powerful and easy to develop with Tendermint BFT and the modularity of the Cosmos SDK.</li>
	<li>Cosmos enables blockchains to transfer value with each other through IBC and Peg-Zones, while letting them retain their sovereignty.</li>
	<li>Cosmos allows blockchain applications to scale to millions of users through horizontal and vertical scalability solutions.</li>
</ul>

https://v1.cosmos.network/intro

## =================== What is Aptos ? ===================

The Aptos network is comprised of nodes of three types: validator node, validator fullnode and public fullnode. To participate in consensus, you are required to run both a validator node and a validator fullnode, and stake.

Grabbing APTOS CLI:

```bash
	$ curl https://github.com/aptos-labs/aptos-core/releases/download/aptos-cli-v1.0.4/aptos-cli-1.0.4-Ubuntu-x86_64.zip -O -J -L
```

## ================= Blockchain Recap 0: =================

A blockchain can be described as a digital ledger maintained by a set of validators that remains correct even if some of the validators (less than a third) are malicious. Each party stores a copy of the ledger on their computer and updates it accoding to the rules defined by the protocol when they receive blocks of transactions. 

The goal of blockchain is to ensure that the ledger is correctly replicated, meaning that each honest party sees the same version of the ledger at any given moment. The main benefit of blockchain technology is the ability for parties to share a ledger without having to rely on a central authority, hence decentralized in nature.

A blockchain is a deterministic state machine replicated on full-nodes that retains consensus safety as long as less than a third of its maintainers are Byzantine.

<ul>
	<li>A state machine is just a fancy word for a program that holds a state and modifies it when it receives inputs.</li>
	<li>Deterministic means that if you replay the same transactions from the same genesis state, you will always end up with the same resultant state.</li>
	<li>Consensus safety refers to the fact that every honest node on which the state machine is replicated should see the same state at the same time. When nodes receive blocks of transactions, they verify that it is valid, meaning that each transaction is valid and that the block itself was validated by more than two thirds of the maintainers, called validators. Safety will be guaranteed as long as less than a third of validators are Byzantine, i.e. malicious.</li>
</ul>

A consensus mechanism such as proof of work (PoW) or proof of stake (PoS) secures the network and prevents unauthorized users from validating bad transactions. To achieve this, participant nodes must prove that the work they've done, giving them the right to add new transactions to the blockchain. 

<ul>
	<li>in PoW (Blockchain 1.0) -> we call them (participant nodes) Miners.</li>
	<li>Miners must solve a complex mathematical problem by finding a cryptographic hash of a particular block. This is done by taking data from a block header as an input, and continuously running this data through a cryptographic hash function. Every time this is done, small changes are made to the input data by including an arbitrary number called a nonce.</li>
	<li>in PoS (Blockchain 2.0) -> we call them Validators (also are fullnodes), fullnodes are only for RPC -> Validators are used for validation (though they are also fullnodes).</li>
	<li>Crypto validators lock up, or stake some of their coins in a wallet. They then validate blocks if they discover a block that can be added to the blockchain. Validators get a reward (or their stake increases) proportionate to their bets based on the blocks added to the blockchain.</li>
	<li>Despite this advantage, the PoS algorithm has a serious drawback. The mining capacity of a validator depends on the number of tokens they have, so a miner who starts with more coins gets more control over the consensus mechanism. Additionally, a few miners can purchase many coins, further diluting the mechanism and reducing the system's decentralization property.</li>
</ul>

A validator (also known as block-creator nodes) is a node that proposes a block to the network of validators that run the protocol, executes transactions in a block, and commits the block.

Note: They’re a lot of learning curves with validation. Still, the one that caused the most annoyance is that every chain has a different method of installing its binary and repositories.

