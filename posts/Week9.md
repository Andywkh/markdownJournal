# Week 9 [27022023 - 03032023:]

## ============ Rust for High Performance Projects ============

Choose Rust when:

<ul>
    <li>You care about performance.</li>
    <li>You want fine-grained control over threads.</li>
    <li>You value memory safety over simplicity.</li>
</ul>

Performance

<ul>
    <li>Basically similar to C++</li>
</ul>

Seems to be a modernized version of C++, similar performance but with better syntax to improve readability and parallelization. 

Note to self: dont understand google docs, need to re-read C++, Java and Golang. Recap CS fundamentals.

### ------------------- Rust vs Golang wrt. Sustainability Computing -------------------



## ===== Deploying a Cosmos Validator Node on a GKE Cluster =====

```bash
    $ cd /tmp

    $ wget https://go.dev/dl/go1.19.6.linux-amd64.tar.gz

    $ tar -C /usr/local -xzf go1.19.6.linux-amd64.tar.gz

    $ ls /usr/local/go

    $ export GOPATH=$HOME/go

    $ export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin

    $ exec $SHELL

    $ go version
```

Note: check golang version at https://go.dev/dl/ . Alter URL code above as required.

https://buildvirtual.net/how-to-install-go-on-ubuntu-20-04/

A genesis file is a JSON file which defines the initial state of your blockchain. It can be seen as height 0 of your blockchain. The first block, at height 1, will reference the genesis file as its parent. When you create your own genesis file, it also initializes an app.toml, client.toml, and config.toml. And with these toml files, we are able to change our validator.

<ul>
    <li>Config.toml contains all the configurations for our node.</li>
    <li>App.toml contains all the specifications for how we handle the state of the chain we validate.</li>
    <li>Client.toml defines the methods in which we export consensus operations, store keys, and broadcast transactions.</li>
</ul>

In our data directory, we store application data, priv_validator_state.json and much more:

<ul>
    <li>Application and blockstore database is axiomatic.</li>
    <li>The priv_validator_state.json stores what round and what step in that round the last signing operation occured.</li>
</ul>

When we have installed the newly updated genesis file, we’ll need to sync our node to the chain’s current block height. There are three ways to do this, the first is tedious, and the second/third involve trust but are multiple times more efficient.

- Syncing to the chain from 0. -> benefit of having archival data, but takes a long time.

    - Archival Node: has all historical state, saves IBC transactions, and contains the information needed to be a valid node ofthe chain.

    - The Pruned Node: contains only the information needed for a node to be a valid instance of the chain.


- Syncing to the chain through a snapshot; A snapshot is the minimal application state needed to sync to the chain. The node will contain no historical state from previous heights, and the snapshot will be added to the database. To attain the snapshot, you have to download it from a third party.

- State-sync is very similar to a snapshot in that they fetch the same data. The two processes go about it in different ways. State-Sync fetches data through peers on the chain versus the second option.

    - When state-syncing, our node must verify the light client it is pulling from. It’ll need an RPC server, a trusted height, and a Block ID hash of the trusted height.

When we are altering our config file (config.toml) to contain a list of peers. At the node’s initialization, it will dial (call) a set of seed or Persistent peers.

<ul>
    <li>Seed: Connects and queries other peers’ address books to update the persistent peers in the node’s local address book and then disconnects.</li>
    <li>Address Book: Contains a list of peers the node tries to connect with.</li>
    <li>Persistent Peers: the peers that the node makes an effort to connect to permanently. If the connection fails, the node will redial. Peers are used to pass information to or receive information from.</li>
</ul>

https://hub.cosmos.network/main/validators/validator-setup.html

https://medium.com/oregon-blockchain-group/creating-a-cosmos-validator-from-0-to-hero-f8b2426fda5c

### --------------------------------- Running TMKMS ---------------------------------

```bash
    $ apt update

    $ curl https://sh.rustup.rs -sSf | sh <press 1 proceed with installation>

    $ source "$HOME/.cargo/env"

    $ rustc --version

    $ apt install build-essential

    $ gcc --version

    $ apt install pkg-config

    $ apt install libusb-1.0-0-dev
```

https://github.com/tomtau/tmkms/tree/feature/nitro-enclave

https://www.fortanix.com/blog/2022/05/securing-tendermint-apps-with-fortanix-dsm-tmkms

https://docs.osmosis.zone/osmosis-core/keys/tmkms/

https://github.com/cosmos/cosmos-sdk/issues/5720

https://stackoverflow.com/questions/61340917/cosmos-sdk-remote-connection-refused

## ================= Blockchain Recap 1: =================

## Learning Courses Completed
- Udemy - Ethereum Blockchain Developer Bootcamp With Solidity (2023)

What is a Smart Contract?

- A piece of code running on the blockchain (deployed as EVM Bytecode)

    - It is a state machine.
    - Needs mining/transactions to change states.
    - Can do logic operations.

- It is turing complete (meaning, it can solve any computational problem in theory)

Solidity – View and Pure Functions;

<ul>
    <li>view indicates that the function will not alter the storage state in any way. But it allows you to "view" it or read it</li>
    <li>pure is even more strict, indicating that it will not even read the storage state.</li>
</ul>

Solidity - Maping of Mappings;

<ul>
    <li>This structure is used to keep track of token approvals. Example: "Alice (1st address) approves Bob (2nd address) to spend 100 (uint) of her tokens".</li>
    <li>A more common case than "approvals between people" is usually an approval from a person to a DApp. For example: "Alice approves Uniswap to pull 100 USDT from her wallet." And Uniswap is programed to take her USDT only at the moment when she's buying some other tokens against USDT.</li>
</ul>

https://stackoverflow.com/questions/71522649/is-there-any-real-world-example-of-nested-mappings-of-solidity


