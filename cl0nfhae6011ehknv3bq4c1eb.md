---
title: "#100DaysOfWeb3: Decentralized Application Development"
datePublished: Sat Mar 12 2022 05:49:28 GMT+0000 (Coordinated Universal Time)
cuid: cl0nfhae6011ehknv3bq4c1eb
slug: 100daysofweb3-decentralized-application-development
canonical: https://sharonjebitok.notion.site/Decentralized-Application-Development-eb999543931246fdb5f45da3aa6a8237
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1647064154187/lHTP5oPrX.png
tags: app-development, ethereum, smart-contracts

---

A decentralized application(dApp) is a software application that connects smart contracts with UI. They’re backend codes deployed to the blockchain networks. Frontend hosted in decentralized storage like [Filecoin](https://filecoin.io/), [IPFS](https://ipfs.io/) among others.

the characteristics of dApps: decentralized, deterministic, turning complete, and isolated. The dApp backend communicates with the Ethereum blockchain and can't be changed.

## Why dApp Development

***Benefits of Dapps development***

- Zero-downtime: once deployed on blockchain the network as a whole is able to serve clients looking to interact with the contract
- privacy: you don’t need to provide real-world identity to deploy/interact with dApp
- resistance to censorship: no single entity on the network can block users from submitting transactions
- complete data integrity: Data stored on the blockchain is immutable & indisputable thanks to cryptographic primitives
- trustless computation/verifiable behavior: smart contracts can be analyzed & are guaranteed to execute in predictable ways without the need to trust a central authority

***Drawbacks of dApps***

- maintenance
- performance overhead: scalability
- network congestion: nodes are interrelated if one has an issue it will affect the other plus difficult computations
- user-experience: low adoption of web3 due to bad user experience...you can’t interact with a smart contract without frontend
- centralization: networks are not fully decentralized

## Types of Decentralized Applications

***Decentralized Finance(DeFi)***
- [Uniswap](https://uniswap.org/)
- [AAVE](https://aave.com/)
- [ALEX](https://www.alexgo.io/)
- [Dao project that takes project ides](http://bullperks.com)

## Development Environment Setup Options

***Ethereum IDEs***

- [Remix](http://Remix.ethereum.org): widely used but try other IDEs find one that is flexible for you
- [EthFiddle](http://Ethfiddle.com)
- [ChainIDE](https://chainide.com/)

***writing a smart contract***

- solidity [documentation](https://docs.soliditylang.org/en/v0.8.11)
- declare the license (identify the owner/standard) - use any license i.e MIT. without it, it will plug an error...have a license
- declare solidity version of code: pragma ^0.8.11 (use the latest version). Highly dependant on contracts you’re using some could have been declared absolute for older versions
- state contract: the contract is like a class in other program languages. declare the name of contract: voting system have a ballot.
- struct is like a user-defined variable(state & local variable -  global). Struct is like a custom variable that has a data type of an address. uint. Within a struct, we have other data types.
- constructor input value before deployment: top priority functions should be inside a constructor's

Thank you for reading through my article on this series. I will be dropping more articles linked to my **100DaysofWeb3** journey. You can leave a comment or suggestion. We can also connect more on [Twitter](Twitter.com/SharonJebitok) or [LinkedIn](LinkedIn.com/in/sharon-jebitok)