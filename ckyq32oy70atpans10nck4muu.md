---
title: "Blockchain Development Tools"
datePublished: Sat Jan 22 2022 17:06:06 GMT+0000 (Coordinated Universal Time)
cuid: ckyq32oy70atpans10nck4muu
slug: blockchain-development-tools
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1642871146902/Qofcj9MyI.png
tags: ides, nodejs, devtools, blockchain, developer-tools

---

This article covers blockchain development tools it is a continuation of my [previous article](https://jebitok.hashnode.dev/decentralized-applications-ckycdbcsp00ggths1362m07aa). 
## Remix
[Remix](https://remix.ethereum.org/) is an online IDE for developing Ethereum smart contracts. It is easy snd fast to start developing smart contracts using it. It is a solidity IDE that has a menu, code editor, plugins, autocomplete, buttons, and test-net refunded with fake ETH.
## Nodejs and Npm
Most development tools are built with node.js. Installing [node.js](https://nodejs.org/en/) comes with npm needed in the installation of blockchain development tools, and dependencies. Alternatives are [asdf](https://github.com/asdf-vm/asdf-nodejs) and [nvm](https://github.com/nvm-sh/nvm) which are version managers.
## Ganache
It is a local development environment i.e local Ethereum blockchain. [Ganache](https://trufflesuite.com/ganache/) is a simple tool and it's made by creators of [truffle](https://trufflesuite.com/). It is a developer-friendly Ethereum client designed to run Ethereum blockchain locally. 
Ganache runs on your machine i.e isolated test-nets i.e offers a safe sandbox, free to exponentiate i.e make mistakes without real-life consequences. Ethereum addresses are pre-funded with fake ETH.
### Ganache CLI vs Ganache GUI
[Ganache](https://trufflesuite.com/ganache/) CLI & GUI versions offer a nice visual interface on top of CLI but require slightly more work to integrate into your Truffle project which is less flexible.
Installing Ganache: version of Ganache CLI comes installed on Truffle. Install Ganache GUI using the npm package 
```
npm install -g ganache-cli
```
You can also install the Ganache GUI installer of your operating system.  Ganache has advanced features like chain forking. 
### Truffle
[Truffle](https://trufflesuite.com/) is a framework for developing smart contracts on the Ethereum blockchain. It's compatible with solidity and Viper which are the main languages for writing smart contracts on the Ethereum blockchain. 
Truffle is written in node.js and it's installed using the npm package
```
npm install -g truffle
truffle init
```
Just like [Remix](https://remix.ethereum.org/), Truffle can also be used to develop more advanced smart contracts, test smart contracts and develop full decentralized applications i.e smart contracts & front-end but on truffle projects, truffle doesn't manage the frontend.  
- [Truffle vs Remix](https://ethereum.stackexchange.com/questions/6897/what-is-the-difference-between-truffle-and-remix)
### Metamask
It's a popular Ethereum wallet. It allows user to store their ETH or other tokens that are ERC20 and ERC721 compatible. Most dApps expect users to have a Metamask wallet in order to interact with their smart contract.
Metamask offers users private keys that allow them to sign in in order to make transactions, create accounts and keep their wallets safe. 
dApps never touch private keys directly they only ask users to confirm transactions with a pop-up. Metamask is available as a distributed browser extension and also a mobile application.
### Infura
[Infura](https://infura.io/docs) is an Ethereum API that allows:-
- easy deployment of smart contracts to main-net and public test-nets
- sending transactions to these nodes
*how to use Infura*
- create a project in Infura
- fund address faucet
- add configuration for deployment
- if you're running deployment with Truffle create an Infura account
### EtherScan
[Ethercan](https://etherscan.io/) is a blockchain explorer for Ethereum that allows users to explore what is happening in the Ethereum blockchain. 
It allows you to:-
- inspect address balanced
- inspect transactions
- read the data of smart contract
- [AM Token Tracker](https://etherscan.io/token/0x121bae03f1fba9235a6bd34d1b902fb812b3b0c7) verify that code of smart contract for developers
Transactions inspection feature independently verify transactions processed on the ETH network. Standard practice once you send a transaction your wallet like Metamask shows Etherscan link of transaction.

Thank you for reading through my article you can leave a comment or suggestion. In the next, we'll cover introduction to smart contracts and solidity development. We can connect more on [Twitter](https://twitter.com/sharonjebitok) and [LinkedIn](https://www.linkedin.com/in/sharon-jebitok/)