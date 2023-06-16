---
title: "Introduction to Decentralized Applications(dApps) : Part 1"
datePublished: Mon Feb 22 2021 19:40:04 GMT+0000 (Coordinated Universal Time)
cuid: cklsf3ftd032i10s180840mmn
slug: introduction-to-decentralized-applicationsdapps-part-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1614713807383/ReWOw727dy.png
tags: blockchain, ethereum, smart-contracts

---

> "Article inspired and written based on teachings of BSC MasterClass: African Developer training conducted by Tim@WhiteMatrix, ChainIDE, White Matrix." 

As per the previous articles on our [blockchain series](https://hashnode.com/series/blockchain-ckklhkpg40001a6s1hmop8uxd/) we saw an introduction to blockchain technology and that it's based on:- a public database that is updated and shared across computers in a network i.e distributed database, for a block the data and state are stored in sequential blocks. Basically, the fundamentals of blockchain technology are:- proof-of-work, consensus mechanism used by blockchain networks, mining i.e miners solve a difficult puzzle to add a new block to the chain, broadcast of new blocks in the network are checked, verified and state is updated for everyone. We also touched slightly on P2P trading to decentralized computation which online payments are made between two parties(buyer & seller)and the decentralized computation comprises the proof-of-work validation process.

*Overview*

- P2P trading to Decentralized Computation
- introduction to Ethereum and Binance Smart Chain
- introduction to dApps
- writing and deploying a simple dApp in ChainIDE

**Terminologies**
- *Blocks*: a batch of transactions that are immutable, complete cryptographic verifiable
- *Smart Contracts*: a reusable snippet of code that a developer publishes into the EVM memory.
- *Decentralized Application(dApp)*: an application that uses smart contracts as its backend.
- *Blockchain*: a sequence of all blocks that have been committed to the network in the history of the network.
- *ETH/BNB*: native cryptocurrency of Ethereum & Binance Smart Chain(BSC) network(s).
- *Blockchain Virtual Machine*: the global virtual computer whose state every participant on the blockchain network stores and agrees on. Any participant can request execution of arbitrary code on the BVM, code execution changes the state of the BVM. e.g EVM and customized Docker Environment.
- *Node*: real-life machines storing the blockchain VM state.
- *Accounts*: user identity on the blockchain network, where ether is stored.
- *Transactions*: request of code execution on the blockchain VM i.e for user A to sender some ether to user B, write smart contract code into EVM memory, execute the smart contract code at address A in the EVM with arguments of B.

## Smart Contracts

We need smart contracts beyond  financial transactions: Decentralized P2P Tx Engine i.e Decentralized P2P TX and as computation engine/platform

examples: Ethereum, BSC(Binance Smart Chain), Polkadot

### Ethereum

Ethereum is an open-source, public, blockchain-based distributed computing platform. According to the Ethereum whitepaper, its Smart contract (scripting) functionality facilitates online contractual agreements.
 ### Computation  via Blockchain Virtual Machine
In the blockchain network i.e Ethereum or BSC,  there's a single canonical computer e.g Ethereum Virtual Machine(EVM), that everyone on the network agrees on its state and everyone keeps a copy of the state on this computer. Everyone can also perform arbitrary computation and broadcast it. All other nodes will:- verify, validate and execute this computation, and the state change in the EVM will be committed and propagated throughout the entire network.

Features of computation on BVM
1. *Cryptographic*:- Transaction is signed by the original executor to ensure all transactions are executed with appropriate permission 
2. *Immutable*:-  Once transactions are verified as valid and added to the blockchain, they can not be tampered with.

### Native Crypto-Currencies
- This comprises Ether & BNB i.e the official cryptocurrency of the blockchain this is for the existence of a market for computation and also provides an incentive for participants to execute and verify transactions.
- There's an offer of some ether when one send/broadcast a transaction i.e award whoever verifies, executes or commits the transaction to the blockchain.
- The amount is based on a function of the length of computation i.e gas in Ethereum.
### What are dApps

*dApp* is backend code running on a decentralized P2P network and it has a frontend code and user interfaces implanted on any programming language.
It's the decentralized Applications that developers upload programs into canonical virtual machines. e.g EVM. All users on the blockchain network can execute functions of those programs i.e deploy a set of functions (smart contracts).

Blockchain is used in business logic and the data layer. Any user can interact with this arbitrarily complex user interface and services i.e marketplaces, financial instruments, and games.
They're built on a decentralized network with a smart contract and front-end user interface. DApp can also comprise of Open API i.e smart contract implemented and developed by others.

*Features of dApps*
- Decentralized i.e independent and no one controls them.
- Deterministic i.e same state, same action & same output.
- Turning complete i.e dApp can perform any action.
- Isolated i.e bug of a smart contract can't tamper with the entire network.

*Benefits of dApps*
- Zero downtime i.e network as a whole will always be able to serve clients.
- Privacy i.e no real-world identity
- Resistance to censorship i.e data on the blockchain is immutable & indisputable
- Trustless computation and verifiable behavior i.e smart contracts can be analyzed and guaranteed to execute in predictable ways without the need for the trust of central authority.

*Issues with current dApps*
- They are harder to maintain
- Has a huge performance overhead and it's hard to scale up.
- There's network congestion, the network can only process 10 transactions per second.
- It's hard to engineer user-friendly experiences.
- Centralization i.e user-friendly developer solutions are built on top of the base layer of Ethereum.

*Centralization vs Decentralization*

Centralized systems have:-
-  a low diameter
- better performance 
- its ultimate source of truth is the central authority, which controls participation on the network, and it can censor data
- has a single point of failure
- coordination among network participants is much easier

Decentralized systems have:-
- Information broadcast might take a lot of time
- has worse performance i.e TPS, throughput
- Protocol is needed to dispute a resolution i.e no central authority and hence censorship is harder.
- coordination is difficult
- anyone can participate on the network


### Ethereum/BSC Accounts
Ethereum/BSC account receive, hold, and send ETH/BNB and token as well as interact with deployed smart contract. Ethereum account can either be:-
- an *externally owned account* i.e owned by individuals/organizations and controlled by private keys. Creating an account costs nothing, one can initiate transactions that can only be ETH transfers if it's between externally owned accounts.
- *contract account* i.e autonomous accounts that are controlled by code (smart contract deployed on the network). Creating an account is costly since you're using network storage, transactions are only sent in response to receiving a transaction whereas transactions from an external account to a contract account can trigger code.

*Fields of Ethereum Account*
- nonce i.e a counter that indicates the number of transactions sent from the account and it ensures that transactions are only processed once. For a contract account, nonce represents the number of contracts created by the account. 
- balance i.e represents the number of *Wei* owned by this account. Wei is a denomination of ETH and there are 1e + 18 Wei per ETH. (Just like satoshi for bitcoin)
- storage hash - account storage 
- code hash on the EVM

In the next article, we'll continue with the Ethereum Accounts, more on smart contracts, and how to create a simple smart contract using Chain IDE and deploy/test using BSC Testnet & Metatask.
























































