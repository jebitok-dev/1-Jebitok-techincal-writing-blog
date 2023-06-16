---
title: "What's Decentralized Finance and How to build a DeFi App"
datePublished: Wed Apr 14 2021 21:26:54 GMT+0000 (Coordinated Universal Time)
cuid: cknm95kh107uzkos114na8quz
slug: whats-decentralized-finance-and-how-to-build-a-defi-app
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1618694676561/F-lW9orus.png
tags: blockchain, ethereum, smart-contracts

---

## Part 1

### Disruptive Innovation Decentralized Financial Innovation 
Disruptive innovation in finance.
CEFI refers to a traditional finance system where users trust their funds to third parties i.e banks, country/governments. For example Binance, CoinBase and Kraken

DEFI refers to a movement that champions the provision of peer-to-peer (P2P) financial services. DEFI solution gives users greater control over their transactions. They achieve this by eliminating centralising authorities from the execution. For example  Compound, Maker and Uniswap.

### Traditional Financial System 
For example, banks have branches that have to communicate to the main centre daily and each branch has independent and centralized ledger tracks within their financial system. There's synchronisation between banks branches and centre. In case one wants to transfer money from Europe to Africa bank branches have to communicate with the centre. 

### Decentralized Financial System
Ledgers are decentralized and distributed, they held by all on the network eliminating the need for a central authority. 

### CEFI vs DEFI
 CEFI has a central authority, traditional database, third party involvement, lack of transparency, customer support and KYC policy for most.

DEFI has no central authority, users have full control, it's based on blockchain technology, no third party involvement, its transparency, permissionless and less/ no customer support.

With Defi, anyone with internet and devise can have access to financial products whereas CEFI can be limited to those with ID/ passports etc, age limit, bank account etc.

### Case products areas 
- For every CEFI there's a corresponding product in blockchain for example:
- Compound offers banking/lending services on blockchain 
- Derivatives/option services eg betting, freelancing etc
- There's a lot of wallets that stores crypto & NFTs just as in the case of bank accounts in CEFI
- Insurance /Covers
The blockchain ecosystem is complete and there's space to extend it.

### Total Value Locked(TVL)

$32.7 B Fast-growing TVL which locks ERC20 token on project and project gives interest. More TVL reflects the success of the project. TVL increased 1BVN to 35BVN

**Trading Volume**
- Centralized Exchange (CEX)
- Decentralized Exchange (DEX)
Trading volume grows rapidly in August 2020 Uniswap DEX was at 1BVN per day and has grown to 35BVN/day in terms of trading tokens. Gross of Defi market reflects more people on Defi ecosystem.
### DeFi Conceptions 
It has 5 layers i.e 
- *Settlement Layer*

Consists of blockchain and native asset i.e bitcoin. For ownership information and ensures follows bitcoin rules.

- *Asset Layer*

Assets used on top of the settlement layer i.e nature protocol, NFTs or other assets(ETH).

- *Protocol Layer*

Specific use cases of Defi: compound, maker and Uniswap. On-chain asset management implemented as a set of smart contract and can be accessed by users through an exchange, lending, derivatives and asset management.

- *Application Layer*

User-oriented application service based on individual protocol. Having a smart contract (backend) but need a frontend for users to interact with the app. Achieved through web development(frontend: HTML, CSS and web3js). 

- *Aggregation Layer*

How to interact with protocol made on protocol layer e.g lending protocol for making better choice aggregation provides solutions.

The best thing about Defi is that it can be put into the chain without maintaining it. Only pay fees and contributions to update Defi products.

### DeFi products 

**classification**

- Credit & lending
- DEXs
- Stable coins
- Derivatives
- Asset management
- Infrastructure
- Data services
- Aggregator

The classification changes over time. In 2020 a lot of projects emerged, here are some examples:-

January, 2020:
> - *Curve Finance* launch 
- *AAVE* launched of Mainnet

May 2020:
> - Uniswap V2

June, 2020: 
> - Compound Liquidity mining

July 2020: 
> - Launch of Yearn Finance

August 2020: 
> - Sushiswap forked from Uniswap

September 2020:
> - Uniswap airdrop 
- Empty set dollar launch

November 2020: 
> Launch of cover protocol

December 2020:
> - Ethereum 2.0
- AAVE V2 launch
- graph launch
- 1inch Airdrop 

### [Uniswap]()

It's the first decentralized trading platform that processes over $100 B in volume i.e existing milestone for Defi. 

It was founded by Hayden Zadan who is the founder/creator, he was previously a mechanical engineer but learnt solidity on his own and Buterin learnt about him and sponsored him.

Uniswap Pool
It stores two tokens i.e ETH & DAI. One deposits i.e Liquidity providers. The smart contract has flash swaps and Oracles. ``Traders -> Swap -> Traders``

Liquidity by putting some token(ETH/DAI) pool has real-time rate(1 ETH/400 DAI) to Uniswap pool. Traders can trade by pool sell 1ETH pool gets the 1ETH and gets back DAI to the trader.

*Advanced Features*

- flash swaps i.e do transactions in one block to lend/trade in a cheaper way.

Oracle provides information on a real-time rate on Uniswap using the constant ``K`` formula i.e real-time rate ETH/DAI rate.

```
X  x Y = K (constant)

1 ETH(X) 400 DAI (Y)
X     Y  and it turns into 2 ETH on the pool.

2  400 / 2 = 200  400
2  200 DAI            400
Constant  doesn't change so 1 ETH equal 200 DAI
```
Trading on pool ETH  and DAI. Capacity pool influence spatulate of the transaction price of ETH/DAI 1/400 changes to 1/200. This does harm to the trader or harm to liquidity provider hard to balance.
 
### Maker
Mint stable coin/asset. DAI i.e stable coin minted from more than 100% of synthetic assets.

```
1ETH makerDAI 100 DAI

Deposit 
```
### Introduction to Farming System & how to Build
- It's based on three projects: 2 ERC-20 tokens contract and DAI Token Contract. Users can stake DAI and get DAI. DAI Token contract i.e dApp token which users stake(transfer) DAI and get DAI