---
title: "Understanding Blockchain, Crypto, NFTs, Smart Contracts, Web 3.0 from Hacker’s View"
datePublished: Fri Jul 01 2022 03:43:57 GMT+0000 (Coordinated Universal Time)
cuid: cl51wvfaf01hfxbnvfq2ea1vx
slug: understanding-blockchain-crypto-nfts-smart-contracts-web-30-from-hackers-view
canonical: https://sharonjebitok.notion.site/Understanding-Blockchain-Crypto-NFTs-Smart-Contracts-Web-3-0-from-Hacker-s-View-90b3af7858854e678e8b7a7ed8405136
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1656646909661/V0I--xat2.jpg
tags: security, blockchain, smart-contracts, blockchain-security

---

This week I attended a session by [Security Weekly](https://twitter.com/SecWeekly) Podcast on [YouTube](https://www.youtube.com/securityweekly) where they touched on Hacker’s view on today’s emerging tech and security. A session by [Tyler Robinson](https://twitter.com/tyler_robinson), [Beau Bulock](https://twitter.com/dafthack), and [Paul Asadoorian.](https://twitter.com/SecurityWeekly) 

### History of Blockchain

2009: Bitcoin launches as the first public ledger for transactions using blockchain.

2015: Ethereum launches and adds smart contracts functionality

2017: First decentralized application(dApp) gains traction

2018: Decentralized Finance(Defi) protocols

2021: Blockchain is used for supply chain tracking, digital identity, anti-piracy, NFTs, and real estate amongst others

## Why Blockchain Security

### Blockchain Hacks Result in Huge Monetary Loss

- Centralized Exchanges: over $2.5 Billion has been stollen since 2011
- Smart Contract Hacks: $ 1.2 Billion was stolen in 2021 and over $1.5 Billion has been stolen in 2022 so far.
- Consumer Wallets: Millions lost through social engineering and remote exploitation

### Why So Many Hacks

Blockchain technology is an emerging technology and many organizations are racing to be “first to market”. Consumers are uninformed of risks and traditional security protection tends to be an afterthought. 

### Blockchain Ecosystem

- Layer1: underlying blockchain protocol itself for example Bitcoin Core, GEth
- Layer2: an overlying network on top of Layer1 typically focused on scalability for example Bitcoin Lighting network
- Smart contracts: they automatically execute programs deployed to the blockchain e.g Tokens, NFTs, dApps
- Software Wallets: custodial wallets vs non-custodial wallets
- Hardware wallets: physical devices for storing private keys that are then used to send & receive funds
- Mining software: a program used to run specialized hardware used to perform mining.
- Centralized exchange: typically requires KYC e.g Coinbase, Binance
- Decentralized Exchanges: Defi exchanges are typically built via smart contract, web3 libraries, and frontend
- People: social engineering, rug pull(are scams where investors invest in a project then founders disappear with the funding), and asset protection

### Smart Contracts

- “Once you launch your code there’s little you can do to fix any problems” Mastering Ethereum by **Andreas M. Antonopoulos and Gavin Wood, Ph.D**
    
    

Smart contracts are immutable computer programs that undertake decentralized execution and run on a virtual machine via node software. They are written in a high-level language, smart contracts get compiled into bytecode prior to deployment to the blockchain. Smart contracts are only called if they are called by a transaction.

### Solidity

- It is a high-level programming language for smart contracts. By far it is the most dominant language currently. It has a syntax similar to JavaScript or C++.
- [Remix IDE](https://remix.ethereum.org/) is a browser-based IDE/Development environment used for writing smart contracts. Also, development environments like [Hardhat](https://hardhat.org/), [Foundry,](https://github.com/foundry-rs/foundry) and [Truffle](https://trufflesuite.com/) can be used.
- Deployed code tends to be posted publicly and verified on sites like [Etherscan](http://etherscan.io/), [PolygonScan](https://polygonscan.com/)

### Smart Contract Vulnerabilities Overview

Smart contract vulnerabilities have resulted in a significant loss of funds. Since smart contract code is public anyone can analyze it for issues. Exploits can be tested against private local blockchain instances or on a testnet. 

There’s no patching since contracts are immutable and stolen funds can be very difficult to track. 

## Common Web3 Attacks

### Common Smart Contract Attacks

- Access Control: It governs who can transfer assets, mint tokens, vote on proposals, or freeze transfers among others.
- Oracle Manipulation: Non-blockchain oracles can be leveraged to get real-world data like Weather, Prices, etc. At times these oracles can be abused.
- Reentrancy: vulnerability where a function can be re-entered prior to completion
- Integer overflows & underflows: overflows and underflows can occur by performing arithmetic operations that exceed the maximum or minimum integer values
- Front-Running: miners give priority to higher gas prices so an attacker can have their transaction mined before the original sender by paying more

### Token Approvals

Some of the ERC Tokens have functionality built-in to allow users to have token transfers. Malicious actors can trick individuals into allowing token transfer approvals. In these cases, the approved contract or wallet can send on behalf of the individual and drain their wallet. [Revoke.cash](http://Revoke.cash) or [Etherscan.io](http://Etherscan.io) can be used to remove token approval.

### Administrative Issues

This can influence vulnerability to various security risks depending on how private keys are stored:

- centralized exchanges
- software wallets
- hardware wallets
- browser extensions
- mnemonic seed phrases
- multi-sig wallets

### Social Engineering

Common Web2 security issues still exist in web3 like Phishing, Scams, centralized exchange hacks, and sim swaps. After social engineering, a victim of an attacker will typically drain a wallet and there’s little that can be done to recover these funds.

### Discord Hacks

Discord has become the primary home for most crypto projects. Crypto Discords get hacked all the time:

- Webhook compromise
- Admins getting phished
    - OAuth tokens (session stealing)
    - Cred phishing
- DM scams
- Bot Services compromise

## Start Hacking

### Books

- [Mastering Ethereum](https://github.com/ethereumbook/ethereumbook) by Andreas M. Antonopoulos, Gavin Wood
- [Hands-On Smart Contract Development with Solidity and Ethereum](https://www.amazon.com/Hands-Contract-Development-Solidity-Ethereum/dp/1492045268)
- [Mastering Bitcoin](https://www.amazon.com/Mastering-Bitcoin-Programming-Open-Blockchain/dp/1491954388)

### Solidity

- [CryptoZombies](https://cryptozombies.io/)
- [Solidity By Example](https://solidity-by-example.org/)
- [Consensys Blockchain Developer Bootcamp](https://consensys.net/academy/bootcamp/)
- [FreeCodeCamp Course](https://www.youtube.com/watch?v=gyMwXuJrbJQ)

### CTFs

- [Ethnaut](https://ethernaut.openzeppelin.com/)
- [Capture the Ether](https://capturetheether.com/)
- [Damn Vulnerable Defi](https://www.damnvulnerabledefi.xyz/)

### Bug Bounties

- [Immunefi](https://immunefi.com/)
- [Consensys Bug Bounty List](https://consensys.github.io/smart-contract-best-practices/bug-bounty-programs/)
- [Code 423n4](https://code4rena.com/)
- [Hacken](https://hackenproof.com/)
- [ChainLink](https://hackerone.com/chainlink)
- [The Graph](https://thegraph.com/security/)

### Blockchain Hacking QuickStart Guide

- [BlockchainHAX](https://start.blockchainhax.com/): free guide built by Beau to serve as a quick start guide for hacking blockchain-based technology such as smart contracts

### Key TakeAways

- Use-Cases for blockchain is growing significantly
- hacks result in a significant loss of funds
- the smart-contract hack is still in its infancy
- as new blockchain implementations are developed new hacks will too.

Thank you for reading my article I would like to acknowledge the Security Weekly Podcast Hosts for all the content on this article as was gained through their weekly session. As someone getting started with blockchain security this is of great help and hoping to still use it over time or have it benefit someone else. We can connect more on [Twitter](https://mobile.twitter.com/SharonJebitok) or [LinkedIn](https://www.linkedin.com/in/sharon-jebitok/).