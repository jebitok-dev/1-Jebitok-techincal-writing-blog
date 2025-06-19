---
title: "Blockchain Development Tools"
datePublished: Sat Jan 22 2022 17:06:06 GMT+0000 (Coordinated Universal Time)
cuid: ckyq32oy70atpans10nck4muu
slug: blockchain-development-tools
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1642871146902/Qofcj9MyI.png
tags: ides, nodejs, devtools, blockchain, developer-tools

---

This article covers blockchain development tools, it is a continuation of my [previous article](https://jebitok.hashnode.dev/decentralized-applications-ckycdbcsp00ggths1362m07aa).

## Remix

[Remix](https://remix.ethereum.org/) is an online IDE for developing Ethereum smart contracts. It is easy and fast to start developing smart contracts using it. It is a solidity IDE that has a menu, code editor, plugins, autocomplete, buttons, and test-net refunded with fake ETH.

## Node.js and NPM

Most development tools are built with Node.js. Installing [node.js](https://nodejs.org/en/) comes with npm needed for the installation of blockchain development tools and dependencies. Alternatives are [asdf](https://github.com/asdf-vm/asdf-nodejs) and [nvm](https://github.com/nvm-sh/nvm), which are version managers.

## Ganache

It is a local development environment, i.e, local Ethereum blockchain. [Ganache](https://trufflesuite.com/ganache/) is a simple tool, and it's made by the creators of [truffle](https://trufflesuite.com/). It is a developer-friendly Ethereum client designed to run the Ethereum blockchain locally. Ganache runs on your machine, i.e, isolated test-nets, i.e, offers a safe sandbox, free to experiment, i.e, make mistakes without real-life consequences. Ethereum addresses are pre-funded with fake ETH.

### Ganache CLI vs Ganache GUI

[Ganache](https://trufflesuite.com/ganache/) CLI & GUI versions offer a nice visual interface on top of CLI but require slightly more work to integrate into your Truffle project, which is less flexible. Installing Ganache: The version of Ganache CLI comes installed on Truffle. Install Ganache GUI using the npm package

```python
npm install -g ganache-cli
```

You can also install the Ganache GUI installer for your operating system. Ganache has advanced features like chain forking.

### Truffle

[Truffle](https://trufflesuite.com/) is a framework for developing smart contracts on the Ethereum blockchain. It's compatible with Solidity and Viper, which are the main languages for writing smart contracts on the Ethereum blockchain. Truffle is written in Node.js, and it's installed using the npm package

```python
npm install -g truffle
truffle init
```

Just like [Remix](https://remix.ethereum.org/), Truffle can also be used to develop more advanced smart contracts, test smart contracts, and develop full decentralized applications, i.e, smart contracts & front-end, but on Truffle projects, Truffle doesn't manage the frontend.

* [Truffle vs Remix](https://ethereum.stackexchange.com/questions/6897/what-is-the-difference-between-truffle-and-remix)
    

### Metamask

It's a popular Ethereum wallet. It allows user to store their ETH or other tokens that are ERC20 and ERC721 compatible. Most dApps expect users to have a Metamask wallet to interact with their smart contracts. Metamask offers users private keys that allow them to sign in order to make transactions, create accounts, and keep their wallets safe. dApps never touch private keys directly; they only ask users to confirm transactions with a pop-up. Metamask is available as a distributed browser extension and also as a mobile application.

### Infura

[Infura](https://infura.io/docs) is an Ethereum API that allows:-

* easy deployment of smart contracts to main-net and public test-nets
    
* Sending transactions to these nodes: *How to use Infura*
    
* Create a project in Infura
    
* fund address faucet
    
* Add configuration for deployment
    
* If you're running a deployment with Truffle, create an Infura account
    

### EtherScan

[Ethercan](https://etherscan.io/) is a blockchain explorer for Ethereum that allows users to explore what is happening in the Ethereum blockchain. It allows you to:-

* Inspect the address balance
    
* inspect transactions
    
* Read the data of the smart contract
    
* [AM Token Tracker](https://etherscan.io/token/0x121bae03f1fba9235a6bd34d1b902fb812b3b0c7) verifies the code of smart contracts for developers Transactions inspection feature independently verifies transactions processed on the ETH network. Standard practice once you send a transaction, your wallet, like Metamask, shows the Etherscan link of the transaction.
    

Thank you for reading through my article. You can leave a comment or suggestion. Next, we'll cover an introduction to smart contracts and solidity development.