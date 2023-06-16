---
title: "Introduction to Blockchain Basics & Smart Contracts"
datePublished: Sun Jun 12 2022 08:55:37 GMT+0000 (Coordinated Universal Time)
cuid: cl4b2n14007dgr3nvgedx334d
slug: introduction-to-blockchain-basics-and-smart-contracts
canonical: https://www.youtube.com/watch?v=gyMwXuJrbJQ
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1655024850567/X6rZBecD5.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1655024666832/BPotKP08D.png
tags: blockchain, bitcoin, ethereum, web3, smart-contracts

---

Recently FreeCodeCamp in collaboration with Patrick C Collins released a [Full-Stack Solidity Smart Contract development course](https://www.youtube.com/watch?v=gyMwXuJrbJQ) Javascript/Typescript edition which features using Next.js among other Javascript libraries backed with resources on a [Github Repository](https://github.com/smartcontractkit/full-blockchain-solidity-course-js). This follows the previous [Full-Stack Web3 development course Python edition](https://www.youtube.com/watch?v=M576WGiDBdQ) that was released last year if you’re into web3 as a python developer you can opt for that. 

I recently started using the course and have decided to document each section as I continue learning through it as the course is almost 32 hours long and this could help in better understanding and as a way of taking breaks in between.  All these are courtesy of this course and creators plus the content they provided.

### Best Practices

Take breaks and cover your own pace while taking this course and through your journey in the Web3 developer space. 

You can check it up here on Youtube or the resources shared via the [Github repository](https://github.com/smartcontractkit/full-blockchain-solidity-course-js) also you can follow [Patrick Collins on Twitter](https://mobile.twitter.com/PatrickAlphaC) and talk about the course on [Twitter](http://twitter.com), and [Reddit](http://reddit.com), participate in discussions and collaborate on [StackOverFlow](http://stackoverflow.com), [Ethereum StackExchange](https://ethereum.stackexchange.com/), [Github Discussion](https://github.com/smartcontractkit/full-blockchain-solidity-course-js/discussions), [Youtube comment section](https://www.youtube.com/watch?v=gyMwXuJrbJQ), [FreeCodeCamp’s community Forum](https://forum.freecodecamp.org/), [ChainLink’s Discord Server](https://discord.com/invite/aSK4zew), or [Gitcoin](http://gitcoin.io).

Blockchain technology being a fast-changing tech aspect beware of chronological updates like Ethereum is expected to move to ETH 2.0 and a go-to test-net or RPC provider, npm packages or solidity version right now would be different in the coming from the time they created this course. 

“becoming a solidity developer in this system is more than solidity, becoming comfortable in using the tool, to get and give help, is essential and networking is massive and gives tonnes of fun. Participating in Hackathons like [ChainLink Hackathon](https://chain.link/hackathon) and [ETH Global](https://ethglobal.com/), or blockchain-related hackathons on [Devpost](http://Devpost.com) are some of the best practices. To get through the edge of rabbit, you must keep going once you have stepped into the blockchain developer ecosystem.” Patrick Collins

## Blockchain Basics

Bitcoin protocol was the first blockchain use case and it came into existence in 2009 when Satoshi Nakomoto released the [Bitcoin white paper](https://bitcoin.org/en/bitcoin-paper). It’s decentralized and based on peer-to-peer transactions, a superior store of value also considered digital gold and a limited supply i.e 21 M bitcoin. 

[Ethereum](https://ethereum.org/en/) is the next most popular blockchain use case which was released by Vitalik Buterin and his team. In 2015, the Ethereum protocol was made to allow storage of value and decentralized agreements/organizations. This made it a unique idea it was to take the blockchain that made bitcoin great and add decentralized agreement i.e smart contracts to it.

### Smart Contracts

In 1994, Nick Szabo wrote an [introduction to smart contracts](https://ethereum.org/en/smart-contracts/), and in 1996 he wrote smart contracts as building blocks for digital markets built on automatic, cryptographic secure processes.

Unlike traditional agreements between parties that are done using pen and paper, smart contracts are written or agreed on via code. Bitcoin is Turing incomplete and can only store value whereas Ethereum is Turing complete i.e facilitates decentralized agreements

[Smart contracts](https://ethereum.org/en/smart-contracts/#what-are-contracts) are deterministic and can allow external computations through Oracles which are data providers of decentralized on-chain data to build hybrid smart contracts. For example, Chainlink is a decentralized oracle network.

Decentralized Applications (dApp) are decentralized protocols that allow users to interact with a smart contract through a user interface. 

### What is Web 1, Web 2, and Web 3?

Web 1: is the permissionless open-sourced web with static content

Web 2: is a permissionless web with dynamic content where companies run agreements on their servers. 

Web 3: is a permission-less web with dynamic content i.e decentralized censorship-resistant networks run your agreements and code. It’s accompanied by the idea of user-owned ecosystems where protocols interact with your partially instead of product solely.

Everything was done in life is a result of an agreement or a contract and always bided by a promise and more often promises are broken via trust or incentive. Over the years we have experienced hyperinflation in Zimbabwe and Brazil, and the Robin-Hood crisis. Smart contracts are viewed as a solution to hyperinflation. 

A smart contract is an agreement, contract, or set of instructions that is deployed on a decentralized blockchain. Once smart contracts are deployed they:

- are immutable and can’t be altered
- automatically executes
- everyone sees the terms of the agreement

Decentralized collectives execute smart contract code through decentralized blockchain and decentralized oracle networks to get real-world information/data. Trust is established through the decentralized open-source code and smart contract features. Examples of Decentralized collectives are [Polygon](https://polygon.technology/), [Ethereum](http://ethereum.org), [Avalanche](https://www.avax.network/), and [Chainlink](https://chain.link/)

Decentralized exchanges fix the robin-hood crisis through enabling transparency i.e smart contracts to solve society’s critical issues by countering the risk and likelihood of transaction default. 

Paper Guarantees are brand-based and involve high risk, removed transparency, and low yields from interests whereas cryptographic guarantees are math-based and involve low-risk transparency and high-interest yields.

### Other Blockchain Benefits

- decentralized and has node operators
- transparent and flexible especially for facilitating on-chain transactions
- speed and efficiency
- security and immutability
- counterpart risk removal
- trust minimized agreements through hybrid smart contracts

### Blockchain Use-Cases

- Decentralized Finance(Defi)
    - for example, [Uniswap](https://uniswap.org/) gives access to decentralized markets in an easy, secure, and transparent manner.
- Decentralized Autonomous Organizations (DAOs)
    - for example [Developer DAO](https://mobile.twitter.com/developer_dao), and [MakerDAO](https://makerdao.com/) enable voting and governance
- Non-Fungible Tokens(NFTs)
    - for example [Bored Apes](https://boredapeyachtclub.com/), [CryptoPunk](https://www.larvalabs.com/cryptopunks) that act as financial security consisting of digital data stored in a blockchain, a form of distributed ledger

### Transactions

- set up Metamask by [downloading the Metamask browser extension](https://metamask.io/download/) on your browser either Chrome, Firefox, Brave, or Edge.
- Get started to create a wallet
- You’ll get secret recovery phrase that you should never
- You create multiple accounts and add networks. Every network will have:
    - an address that is the public key that you can share in public or use on [Etherscan](http://etherscan.io) to check transactions linked to the address
    - private key linked to your account which you should also keep private.
- you can also activate show testnet so that you can access Testnets like Rinkeby, Mumbai, Ropsten, and Goerli among others which will be helpful when building and deploying smart contracts to the test environments and when you’ll be accessing testnet faucets.

### How Blockchain Works

To understand how blockchain works you can check this [Blockchain 101 visual Demo course on Youtube](https://www.youtube.com/watch?v=_160oMzblY8) and on the [Blockchain101 Demo site](https://andersbrownworth.com/blockchain/) by Anders Brown Worth where you can practice creating hash from any data, creating a block, blockchain, distributed blockchain, and Token.

*Hashing Algorithm* is a function that computes data into a unique hash.

*Hash* is a unique fixed length and string meant to identify a piece of data. They’re created by placing said data into a hash function. The hash on this Demo is generated using the SHA-256 hashing algorithm but Ethereum uses the keccak256 as their hashing algorithm.

![Twitter post - 1 (5).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1655023782452/6qtrqRs0p.png align="left")

*Block* is a list of transactions mined together. It is made up of block number, nonce, data, and the hash generated. 

- A nonce is used in a couple of ways i.e
    - define transaction numbers for an account/address
    - the number used once to find a solution to the blockchain problem

![Twitter post - 2 (4).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1655023996847/kkhWhpNQj.png align="left")

*Blockchain is* decentralized i.e don’t have a single entity of authority that has more than one block each with the block number, nonce, data, prev, and hash. The first block in the blockchain is the Genesis block

![Facebook post - 1 (1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1655023300288/iJ2SAO3fz.png align="left")

*Distributed blockchain* is a blockchain with more than one peer where each peer is a blockchain made up of more than one block with a block number, nonce, data, prev, and hash. 


![Facebook post - 2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1655023269505/6KATxSOwo.png align="left")

A *token* is made up of more than one peer but unlike distributed blockchains, each peer has more than one blockchain that has block number, nonce, Tx(Transactions with amount & address), prev, and hash.


![Facebook post - 2 (1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1655023252956/seD7rafm6.png align="left")

Mining is the process of finding a solution to the blockchain problem for example finding a hash that starts with zeros. A node who are miners in the Proof of Work consensus and Validator for Proof of Stake Consensus are paid for mining blocks

### Signing Transaction

A private key is only known to the key holder and it's used to sign transactions. Signatures are made through message, private key generates a message signature through the elliptic curve digital signature algorithm. With this, you’re the only one who can verify. 

For public-key anyone can verify unlike with private keys, signatures are made through message and public keys to generate a signature.

### Transaction & Gas

- Gas is the cost associated with running a transaction. One can set the limit on how much gas you’ll like to spend for the given transaction. The Base fee is the minimum gas fee to send in your transaction. In every transaction, there’s an amount of gas that can be burnt. For converting the Ethereum Units from ETH to Wei or Gwei you can use [Ethereum Unit Converter](https://eth-converter.com/).
- The number of gas fees get more expensive the more people use the chain.
- [EIP 1559 standard](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1559.md) explains more on the transaction pricing mechanism that includes a fixed-per-block network fee that is burned and dynamically expands/contracts block sizes to deal with transient congestion.

## Consensus Mechanism, Attacks & Scalability

Consensus is a mechanism to reach an agreement, especially in a decentralized system.

### Proof of Work

Ethereum and Bitcoin currently use the Proof of Work Consensus Mechanism. Sybill resistance is done through a computational process called mining where miners find nonce it could be block time through the PoW and Nakamoto Consensus. 

### Proof of Stake

Proof of Stake offers collateral or staking as a way of dealing with Sybil attack i.e node detected by the network conducting fraudulent or malicious activity stands to lose a part of its stake, as well as the right to participate in the future. It's also a scalability solution. Unlike Proof of Work, node operators are called validators. 

Ethereum 2.0's Proof of Stake consensus will deal with environmental impact & allow sharding which is a scalability solution i.e blockchain of blockchains - remove the limit on a number of users affecting the gas fees and blockchain app.

*Layer 1* is any base layer blockchain implementation e.g Bitcoin & Ethereum whereas layer2 is any application built on top of layer1 e.g Chainlink, Arbitrum, and Optimism.

*Rollups:* Optimism and Arbitrum are Rollups that are scalability solutions that send transactions into layer1 like Ethereum. Rollups are also sharded chains that derive their security from layer1 and send their transactions to layer1.

*Side-chains* derive their security from protocols.

### Attacks

- *Sybill attack* is a malicious attack that involves forging multiple identities e.g the user can create many pseudonymous accounts with the aim to gain an undue advantage within a network.
- *51% Attack,* happens when a malicious user in a network acquires control of a given blockchain’s mining capabilities. It implies that the attackers will have more than 50% mining power and can mine faster than everyone else. The attackers can stop the confirmation and order of new transactions.  
    
    *PoW and PoS are Sybil resistance mechanisms and the bigger the blockchain the more secure it is. Consensus is how blockchain decides what the state of the chain is. Sharding and Rollups are solutions to scalability on Layer1. The scalability issue is not enough blockchain space for many transactions to fit into a block leading to high gas fees whereas Gas fees are how much it costs to perform executions on-chain.*
    
    This was a summary from the first part of the [video](https://www.youtube.com/watch?v=gyMwXuJrbJQ) be sure to also get started with the [course](https://www.youtube.com/watch?v=gyMwXuJrbJQ) as it’s more detailed and beginner friendly and to become a good smart contract one has to understand the core blockchain basics. 
    
    Thank you for reading through the article, hoping to make summarizes as I keep learning through the [course](https://www.youtube.com/watch?v=gyMwXuJrbJQ). Be sure to leave a comment, or suggestion or we can also connect more on [Twitter](http://twitter.com/sharonjebitok) and [LinkedIn](http://linkedin.com/in/sharon-jebitok).