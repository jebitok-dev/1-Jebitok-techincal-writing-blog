---
title: "#100daysofWeb3: Transactions & Accounts"
datePublished: Sat Mar 12 2022 05:15:12 GMT+0000 (Coordinated Universal Time)
cuid: cl0ne97tz00y1hknv0tadedzy
slug: 100daysofweb3-transactions-and-accounts
canonical: https://sharonjebitok.notion.site/Week-2-4181c9e7bb1440d3a78a3255adc9bcef
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1647062049629/mfz1L6ABt.png
tags: blockchain, ethereum

---

From the beginning, I joined a program by [Web3Brdige]() for software developers learning smart contract development. The experience has been good so a=far and I had to choose to journal the learning journey into **#100DaysofWeb3** just realized I'm on day 59 and it would be good if I write an article about how my first 50 days have been.

Web3bridge has a well-structured curriculum that is shared at the beginning of the learning process. They offer the best instructors who follow the curriculum closely. We're currently on week nine and if check what we have learned alongside the curriculum you find everything has been covered and even more have been added.

In the first weeks, we were introduced to the basics of blockchain technology by being introduced to:
#Transactions and Accounts

## Transactions
- types of accounts i.e **externally owned accounts(EoA)** that have private keys linked to your address, cost less to own one e.g Metamask account, can only transfer ETH and **smart contract accounts** that are controlled by code, deploy code to generate address, expensive pay gas fees and deploy smart contract, has cost due to network storage(gas/deploy-minimize gas usage) e.g ENS account expensive to own one.
- When creating a smart contract you'll need an externally owned account to make transactions to the smart contract address to trigger code. Gas is tracked both for the smart contract and when transactions are made between one EoA account with another.
- A public address is generated from private keys and externally owned accounts are generated from a cryptographic hash that helps prove that the transaction was signed by the sender & prevents malicious activities. The private key is used to sign a transaction. 
### Fields on Ethereum Accounts
1. **Nonce**: an identifier that shows the amount of transaction i.e 30 transactions. Ensures that transactions are processed once and not multiple times
2. **ETH Balance**: ETH is measured in Gwei(smallest unit of eth). If you want to add ETH to your can add
3. **contract code hash**: Ethereum virtual machine(EVM) pass a hash of code through EVM (special instructions): immutable. Code hash (empty string for EoAs). It’s executed if EVM gets the 
4. **account’s storage route/storage hash**: account addresses are stored in bytes like SHA256, Keccak256.
### Generating Account
- generating an account will create a random public key which is made up of 64 Hexa characters and can be encrypted with a password.
- the public key is generated from the private key using the [elliptic curve digital signature algorithm](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm).
- explanation about [private vs public key](https://www.youtube.com/watch?v=9LtBDy67Tho)

The account isn’t the same thing as a wallet. The wallet is the custodian of an account.

## Transactions

perform an action in an account: transaction i.e cryptographically signed instructions from accounts. Accounts update the state of the ETH account. Externally owned Accounts(EoA)

Transaction: performed by a human and they change the state of EVM before they’re broadcasted into the entire network. Any node can broadcast transactions. A node can be a person sending transactions, receiving, or a computer. It isn’t easy to be a miner as you’re running nodes. Consumes a lot of power.

Here are members of a transaction requires a fee to be mined 

- recipient: node sending transaction i.e transfer value
- signature: identifier of sender i.e their private key
- value: ETH/Gwei
- data(optional)
- gas limit
- gas: maximum priority of gas(tip to miner to mine your transaction faster)

Thank you for reading through my first article on this series. I will be dropping more articles linked to my **100DaysofWeb3**. You can leave a comment or suggestion. We can also connect more on [Twitter](Twitter.com/SharonJebitok) or [LinkedIn](LinkedIn.com/in/sharon-jebitok).