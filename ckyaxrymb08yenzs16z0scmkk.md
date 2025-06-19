---
title: "Introduction to Ethereum Blockchain"
datePublished: Wed Jan 12 2022 02:41:14 GMT+0000 (Coordinated Universal Time)
cuid: ckyaxrymb08yenzs16z0scmkk
slug: introduction-to-ethereum-blockchain
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1641955208875/gjrACZ0H5.png
tags: development, blockchain, ethereum

---

I'm currently learning blockchain development through the EatTheBlock course and thought I would document it through my learning process. This first article covers the basic introduction to blockchain on the Ethereum Blockchain

## Introduction to Blockchain

**Blockchain** is a special kind of database made up of blocks of data that are linked to each other by cryptography. Each block is made up of two bars:-

* metadata, i.e, timestamp reference block, cryptographic signature of all transactions.
    
* transactions, i.e, how there are changes that transfer from account to account or interact with a smart contract.
    

Blockchain is immutable, i.e, cannot modify data that is already on the blockchain. Blockchain network is decentralized and runs on a network of computers, i.e, nodes that form a blockchain network. The blockchain runs on a public network, i.e, made up of distributed databases that run on a private network.

In the mining process, miners add a new block to the blockchain by computing mathematical equations to add transactions to the block and earn a nonce in return.

Ethereum blockchain is a Proof-of-work algorithm that doesn't rely on a third party.

**Pros**: Ethereum includes immutability, decentralization, censorship resistance, and security through the proof-of-work algorithm.

**Cons**: It's slower than traditional databases, less scalable, can't use external API, expensive(gas fees), centralized transactions, these are things blockchain technology is still trying to solve, a public network(no privacy).

## Cryptocurrency

### Bitcoin

Blockchain is used to build cryptocurrencies, e.g, Bitcoin, for the transfer of virtual currencies between different Bitcoin addresses. Through transactions, one can transfer bitcoin, then a block is created that links relevant transactions managed by external addresses called wallets that have no limit of addresses. Bitcoin miners are rewarded, and Bitcoin was created to reduce inflation by limiting it to up to 21M bitcoins.

### Ethereum

* Bitcoin was the first blockchain use case. Vitalik Buterin created Ethereum in 2013 to fix some limitations that Bitcoin had. Ethereum is a second-generation blockchain capable of processing transactions and any arbitrary computations.
    
* Ethereum blockchain technology is based on blocks, i.e, transactions, miners, proof-of-work, and addresses for the transfer of ETH.
    
* Development of smart contracts & decentralized applications on the Ethereum blockchain
    
* EVM(Ethereum Virtual Machine) runs programs, i.e, smart contracts
    
* Ether: code a multi-signature wallet for several people without spending ETH.
    

## Cryptographic Hashes

A cryptographic hash is used a lot in blockchain to identify a transaction. It is a fingerprint that uniquely identifies a piece of data. It is informed by a gibberish string of data.

A cryptographic hash function is used to generate a cryptographic hash, for example, SHA-2, SHA-3. A cryptographic hash function is a way to function interestingly for privacy.

Ethereum uses the [Keccak256](https://ethereum.stackexchange.com/questions/11572/how-does-the-keccak256-hash-function-work) hashing function. As developers, we build on top of blockchain networks, and we don't care about cryptographic functions; we know that they generate cryptographic hashes from data.

You can read more about the difference between [Keccak256 vs SHA-255](https://ethereum.stackexchange.com/questions/11572/how-does-the-keccak256-hash-function-work)

## Ethereum Addresses

Addresses send and receive Ether. When Eth address is created, a private key, i.e, a long number randomly generated and must be kept secret like a password.

Through [Elliptic Curve Cryptography - ECC](https://avinetworks.com/glossary/elliptic-curve-cryptography/), a secret private key generates a public key. The private key to the public key is 128 characters. Private key to public key(128 char) to hash(64 char) to address(42), out of the hash take the last 40 key char, then prefix 0x.

## Addresses

Give out an address to someone else to send Eth. They'll use a private key to send Ether to another address, i.e, sending transactions. Addresses are created by external software called wallets.

## Wallets

Wallets are software that manage addresses of different blockchain software. Wallets are installed and managed by users. There are different kinds of wallets, i.e, desktop apps, mobile apps, browser extensions like Metamask, Phantom, and physical wallets like Nano Ledger.

### Functions of Wallets

* generate addresses by randomly generating private keys, then compute a public key, then derive an ETH address (for most people, one address; therefore wallet can generate millions of addresses with a single private key through [Hierarchical Deterministic (HD) Wallet](https://www.investopedia.com/terms/h/hd-wallet-hierarchical-deterministic-wallet.asp), i.e, BIP32 and BIP39.
    

A single private key equals multiple ETH addresses. Two ETH addresses can't be bonded to a single private key. The possibility of having address coliation, i.e, operating the same address, is rare.

* Software wallet Metamask loses access to a wallet and private key. Copy private key/ [mnemonic phrase](https://docs.safepal.io/safepal-hardware-wallet/security-features/software-security/mnemonic-phrase) to a wallet onto to piece of paper.
    

## Transactions

Transactions are signed packages of data that describe an action you want to take on the blockchain. There are some kinds of transactions:-

* Sending ETH to another address
    
* Execute smart contracts
    
* Create a new smart contract
    

### Fields of transactions

```python
from: your address or that of the smart contract(sender)
to: recipient address
gas: how much you're willing to pay miners to include your transaction on the blockchain
gas price: how much you're willing to pay miners to include your transaction on the blockchain
value: the amount of ETH to transfer from sender to recipient (in WEI, a denomination of ETH)
data: optional field to include arbitrary data
```

### Lifecycle of Transaction

* Set up a wallet to undertake the transaction
    
* build transaction
    
* Sign a transaction using the private key associated with the address
    
* Send the transaction to the Ethereum blockchain
    
* Mine transaction: miners pick transactions and add them to the next block. The lifecycle of the transaction takes 15 seconds. Once the miner adds to the block, you'll see side effects of the transaction, i.e, an update on a blockchain explorer like [EtherScan](https://etherscan.io/).
    

## Smart Contracts

A smart contract is a small program that runs on the Ethereum blockchain. Smart contracts have some properties that make them different from other programs.

### Pros of Smart Contracts

* code is immutable, i.e, impossible to update, and the same as when they're deployed
    
* They're censorship resistant, i.e, no organization can control them, not even the creators of the blockchain
    
* No need for servers, i.e, can deploy on blockchain and forget about them.
    
* very safe, i.e, if they have no bug, it will be hard to be hacked
    
* easy to transfer money, for example, ETH on the Ethereum blockchain
    

### Cons of Smart Contract

* expensive to run smart contracts, i.e, gas fees
    
* very slow to interact with the smart contract
    
* limited capabilities, i.e, can store too much but no complicated computations, hence the need for oracles.
    
* No future scheduling
    
* can't call the API outside the blockchain
    

### How smart contracts work

Smart contracts have ETH addresses, some code, and data on the blockchain. Writing code of smart contracts using a programming language, i.e, solidity, on the Ethereum blockchain.

```python
pragma solidity 0.6.0;

contract simple storage {
     string public data;

   function set(string memory _data) public {
      data  = _data;
   }

   function get() view public returnS(string memory) {
      return data;
   }
}
```

The code is compiled into EVM(Ethereum Virtual Machine), i.e, bytecode. EVM is part of ETH that runs smart contracts. It doesn't know how to run solidity but how to run elementary instructions, i.e, [Bytecode Elementary Instructions - EVM](https://medium.com/authereum/bytecode-and-init-code-and-runtime-code-oh-my-7bcd89065904). Once the smart contract is compiled and has bytecode, create a smart contract on the blockchain by creating a transaction with EVM, bytecode, and sending it to the blockchain. The miner will pick the transaction, and a smart contract will be created.

Interaction with smart contract by creating other transactions by specifying functions of smart contract to call functions to send ETH to other addresses through computation of functions which can call functions of other smart contracts, which can execute other smart contracts on the blockchain. This is so powerful.

## Gas

To send transactions on the Ethereum blockchain, you need to pay a fee, i.e, to reward miners' efforts.

* The sender of the transaction pays for the gas fees, i.e, the owner of the addresses that signed the transaction
    
* The miner receives the transaction fee
    
* The gas is the amount of the fees. When you run a smart contract, you run an elementary operation on the EVM. at operation is computationally intensive, which is quantifiable into gas, e.g, 20000 Gwei. Scores of all of these operations have a total gas cost that's converted into ETH.
    

```python
total cost(ETH) = gas cost * gas price
```

How much are you willing to pay for each unit of gas? If it's low, miners might ignore your transaction If you need it to be transacted faster, you could raise the price. You can check the average cost of gas on the [ETH Gas Station](https://ethgasstation.info/). The wallet determines the maximum gas/gas price you're willing to pay for a transaction.

## Ethereum Accounts vs Contracts

There are two kinds of Ethereum accounts:-

* Externally owned Accounts (EOA)
    
* Contract Accounts
    

### EOA

Externally owned accounts are controlled by humans or by code outside the blockchain. They have a private key that allows signing transactions.

### Contract Accounts

These are smart contract accounts. They don't have private keys. It is controlled by code and can't initiate transactions, unlike EOA first sends transactions and puts in motion a contract account. Contract accounts react to transactions according to the code of the smart contract

### Account Fields

EOA and contract account have the same fields:-

* *Balance*: balance if ETH is associated with the address, both as ETH
    
* *Data*: contract accounts for its data of smart contract, and for EOA, it's empty
    
* *Code*: for contract accounts, it's the code of a smart contract, whereas for EOA it's empty
    
* *nonce*: for EOA, it's the number of transactions that were sent by address. A replay attack is prevented, i.e, sending the same transaction multiple times until funds run out of the address. For contract accounts, a nonce is the number of smart contracts that were created by this address.
    

## Ethereum Network: local, public testnets, and mainnets

Several networks are completely isolated from each other and have an impact on the others. The same address can be used by several networks. The balance of ETH on one network is independent of the balance of ETH on another network, i.e, you can't transfer ETH between these different networks. Each network has a different purpose.

Thanks for reading through my article. This was the overall basis of the Ethereum Blockchain. The next article will cover decentralized applications, some use-cases, and their architecture.