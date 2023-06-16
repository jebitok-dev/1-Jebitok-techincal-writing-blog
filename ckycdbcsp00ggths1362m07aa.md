---
title: "Decentralized Applications"
datePublished: Thu Jan 13 2022 02:44:00 GMT+0000 (Coordinated Universal Time)
cuid: ckycdbcsp00ggths1362m07aa
slug: decentralized-applications
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1642041744090/hkfhf5YV8.png
tags: apps, blockchain, frontend-development, solidity

---

### Introduction
This article is a continuation of [my previous article](https://jebitok.hashnode.dev/introduction-to-blockchain-development-ckyaxrymb08yenzs16z0scmkk). This aims to cover an introduction and understanding of the architecture and some real-life use-cases of decentralized applications(dApps). Smart contracts are cool and not user-friendly, to use them you'll need to use the command line or write an interface/app that sends requests to Ethereum API.

Decentralized applications are a combination of user-interface(UI) for interacting with smart contracts and smart contracts. [Uniswap](https://uniswap.org/) is a good example of dApp on Ethereum blockchain which is a web application built using HTML, CSS, and Javascript with tokens for buy/sell and one has to connect browser extension preferably Metamask for external Ethereum wallet. 

For dApps UI is updated to show what happens on smart contract and this makes it slower as it has to show confirmation of the smart contract.

### Defi & Popular decentralized applications on Ethereum
- [Defi Pulse](https://defipulse.com/) shows Defi projects and stats of total value locked, maker dominance and Defi pulse index.
- [dApp Radar](https://dappradar.com/) allows you to discover, track & trade everything Defi, NFT, and Gaming. 

#### Decentralized Finance
Defi is finance reinvented on the blockchain. Defi is the most active part dApps. [Defi Pulse](https://defipulse.com/)shows the amount of money invested in Defi before the coronavirus crisis was high up to the end of 2021. 
DAI, a stable coin is a financial asset used on most Defi protocols like Uniswap, [Compound](https://compound.finance/) which is a borrow/lend Defi protocol. Defi is a dynamic industry you can build on top of already existing projects. 

Gaming, Gambling, Play-to-earn models, NFTs are other use-cases of dApp that have risen in popularity in the past year and are expected to still grow going forward. With the rise of Metaverses i.e virtual world, we might expect the growth of dApps and overall adoption. 
### Architecture of Decentralized Applications
dApps feel like web applications but have:-
*smart contracts* 
-has data and business logic 
- lives in Ethereum blockchain 
*frontend* 
- made out of HTML, CSS, and Javascript
- interacts with smart contract. 
- parameters of transaction that are passed into browser-extension like Metamask(wallet) and receives prompt to approve a transaction then wallet sends signed transaction to the blockchain. For a case of a hardware wallet, the frontend sends transactions to the hardware wallet itself though this is a special case.

Transaction hash from the blockchain wallet allows users to verify transactions using blockchain explorer like [Etherscan](https://etherscan.io/).
*Backend*
- its optional but a smart contract can have an ordinary backend to help with cache data and ease frontend reading data from the smart contract.
### Development process of Decentralized Applications
- *Create a model of data*. Inside or outside of blockchain have little data on the blockchain since it's expensive to deploy smart contracts and having external servers will be of help.
- *Code smart contract* using solidity and IDE like Remix
- *Test smart contract* using Truffle then make changes & fix bugs
- *Build frontend* which is user-friendly using HTML, CSS, Javascript, and Web3.js and integrate external wallets like Metamask.
- *Deploy smart contract on public Testnet* 
- *Deploy on public Mainnet*

Thanks for reading through my article in the next article will cover blockchain tools and more. You can leave a comment or follow me on [Twitter](https://twitter.com/SharonJebitok) to connect more.
