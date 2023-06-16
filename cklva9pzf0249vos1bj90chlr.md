---
title: "Introduction to dApps: Part 2"
datePublished: Wed Feb 24 2021 19:48:47 GMT+0000 (Coordinated Universal Time)
cuid: cklva9pzf0249vos1bj90chlr
slug: introduction-to-dapps-part-2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1614714519644/fRD6UF7CU.png
tags: blockchain, cryptocurrency, ethereum, smart-contracts

---

In our [Part 1 of this article](https://jebitok.hashnode.dev/introduction-to-decentralized-applicationsdapps-part-1-cklsf3ftd032i10s180840mmn) we covered an introduction to smart contracts, Native cryptos i.e Ethereum & BSC as well as the Blockchain Virtual Machine among more. In this part, we'll dive deeper into the Ethereum account and how to create a smart contract and deploy & test using ChainIDE and Binance Smart Chain's Testnet.

### BSC/Ethereum Accounts

Externally owned accounts and key pairs i.e the accounts are made up of cryptographic keys that can be either public or private. One doesn't cryptocurrency but private keys and funds are always on the Ethereum ledger.

On the P2P network(node-to-node transaction verification), the signer sends data that's stored on a hash algorithm and encrypted using the private keys and the signer can only sign digitally. The digitally signed document is sent through the network a verifier checks if the hash values are equal to show that the transaction is valid and ensures that it's not modified then decrypted using a public key to get decrypted hash. 

*Transactions* are considered as cryptographically signed instructions from accounts. An account initiates a transaction to update the state of the Ethereum Network. The transaction comprises the following information:-
- receiving address
- identifier/signature of the sender
- amount of ETH/BNB to be sent from sender to recipient
- arbitrary data (optional)
- amount of gas units that can be consumed on the transaction
- transaction fees the sender pays per unit of gas.

```
{
from: "0x89205A3A3b2A69De6Dbf7f01ED13B2108B2c43e7",
to: "0x0472ec0185ebb8202f3d4ddb0226998889663cf2",
gasLimit: "100000",
nonce: "0",
value: "1000000000"
}
``` 
Gas is required for smart contract interaction and the gas not used in the transaction is returned
to the user. When transaction is submitted, cryptography generates a transaction hash: ```0x97d99bc7729211111a21b12c933c949d4f31684f1d6954ff477d0477538ff017``` then transaction is broadcasted on a pool with other transactions. A miner must pick your transaction and add it to a block in order to verify the transaction and to mark it as "successful". The transaction will get a block confirmation number. 

*Blocks* are batches of transactions with a hash of the previous block in the chain.

*EVM* from the ledger to the state machine. Ethereum is based on distributed state machines instead of distributed ledgers like for bitcoin & native cryptocurrencies: BNB/ETH. Ethereum can be viewed as a transaction-based state machine and chain of blocks(blockchain) on the viewpoint of implementation, on the view-point of the ledger its viewed as stacks of transactions. Transactions are collated into blocks i.e package of data. 

A Blockchain network is a globally shared, decentralized transactional database. The decentralized node constitutes of Ethereum P2P network. The external actors access the Ethereum world through the Ethereum nodes.

### Nodes and Clients

The network needs a distributed network of nodes to verify blocks and transactional data and one requires a client on your device to run a node.

*Types of Nodes*
*Full Node*
- It stores full blockchain data.
- participates in block validation, verifies all blocks and states.
- serves the network and provides data on request.

*Light Node*
- Stores the header chain and requests everything else.
- Can verify the validity of the data against the state roots in the block headers.
- Useful for low capacity devices, such as embedded devices or mobile phones, which can't afford to store gigabytes of blockchain data

*Archive Node*
- Stores everything kept in the full node and builds an archive of historical states. Needed if you want to query something like an account balance at block #4,000,000.
- These data represent units of terabytes which makes archive nodes less attractive for average users but can be handy for services like block explorers, wallet vendors, and chain analytics

Syncing clients in any mode other than archive will result in pruned blockchain data i.e there is no archive of all historical states but the full node is able to build them on demand.

### Network

*Mainnet*:

It is the primary public production on the blockchain network, where actual-value transactions occur on the distributed ledger. Mainnet ETH is what people and exchanges refer to when discussing cryptocurrency prices.

*Testnet*:

These are networks used by contract developers to smart contracts in a production-like environment before deployment to Mainnet. e.g ChainIDE x Metamask

*Test Faucets*:

Native cryptocurrency on Testnets has no real value, but you need them to interact e.g BNB on Metamask. Most faucets are web-apps where you can input an address to which you request native cryptocurrency to be sent.

Thank you for reading, you can share your thoughts about dApps, EVM, and blockchain technology. In the next part, we'll cover an introduction to smart contracts and solidity. 









