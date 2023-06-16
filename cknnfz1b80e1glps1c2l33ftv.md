---
title: "Explore the world of Defi! Discover the value of Innovation!"
datePublished: Sun Apr 18 2021 17:26:58 GMT+0000 (Coordinated Universal Time)
cuid: cknnfz1b80e1glps1c2l33ftv
slug: explore-the-world-of-defi-discover-the-value-of-innovation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1618766767721/Dkrk0luvu.png
tags: blockchain, smart-contracts

---

**PART 2**
*outline*
- Decentralized Bank Compound 
- Defi Fund Yearn Finance
- Algorithm StableCoin
## Decentralize Bank Compound
Deposit assets gain interest which changes based on demand versus supply of the borrowers and suppliers of money. Estimate interest on compound site, every asset has a unique interest rate.

### Compound Mechanism
Three rows in the moneylender and three rows compound contract i.e give money lender back. Moneylender deposit/DAI whereas money borrower stakes on stake assets to borrow asset i.e stake ETH and borrow DAI, for this case, the stake asset is greater than borrow asset i.e ETH > DAI. 

### cToken 
How to make real-time loans and real-time repayments. Moneylender deposits 1 DAI which equivalent to 40 cToken on compound DAI stake Contract. 
```
rate = (totalCash + totalBorrows + totalReserves) / totalSupply
```
rate of cToken to the asset will change from time to time but in the long run, it shows a steady upward trend. 
 
### Key Terms 
*Total cash* is the amount of asset put into the contract but has not yet been borrowed. 
*Total Borrows* for all borrowers, the amount of DAI that should be repaid(including interest)
*Total Reserves* is the amount of interest put into a smart contract but will be reserved(not used).
```
utilization rate = totalBorrows / (totalCash + totalBorrows)
totalSupply => cDAI | amount
```
### Compund Liquidation
If money borrower stake 3 ETH($600) get 445 DAI($445)
```
stake rate = 0.75
```
As times flow, Tom has $6 interests
```
0.75 * 600 = 450 < 451  = 445 + 6 (Trigger LIquidator)
```
### Liquidator
Liquidator helps Tom repay part of his debts(less than 50%) through DAI. Liquidator pays 200 DAI($200) to help Tom decrease debt. Liquidator gets paid by x1:1 in ETH($200)

After liquidation, Tom's status change to 
```
0.75 * 380 = 285 > 251 = 451 - 200
```
compound partially repaid debts but maker.
### Compound Governance
The compound is managed by a decentralized organized organization(DAO) where proposals are created, voting is active for 3 days, voting ends when time clocks 2+ days it moves from pending - active(defeated) - succeed - queued - executive.
> DAO is popular in most blockchain projects and if you use DAO it can make people believe in the project more. 

### Features and Advantages of Compound
- Real-time loan and real-time repayment no need for one-to-one lending.
- As long as you hold DAI you can receive DAI interest equivalent to a successful loan.
- Utilization rate is the main factor that affects the compound lending rate
- Interest rate and interest are updated, every block which is equivalent to lending interest every block.
- Liquidation mechanism, avoid non-repayment of borrowed money and protect the rights and interest of lenders.

## Defi Fund: Yearn Finance Protocol
Yearn Finance protocol is an aggregator and smart choice in the market i.e maximize Defi by automation. > Origin of Yearn Finance(YFI) is early 2020, the author of Yearn started automating his strategy. Andre Cronje(AC) created this project around the value of automated yield farming strategies $3 - $35500. The value of the YFI system is based on automated strategies. 

YFI allows holders to determine the direction they want protocols to develop. It has acquired multiple projects in the Defi ecosystem (e.g sushi-swamp, cream finance) to form a self-articulating ecosystem.

### Core Products 
*Lending Protocol*: 
```
DAI -> AAVE (7.42%)
DAI -> Compound(5.69%)
DAI -> dydx(5.21%)
```
each project has an interest rate, on how to choose the best every time. 
### Using YFI
deposit money (DAI/wETH/ETH/3CRV) *vault gives the investment strategy which indicates highest either compound, dydx, AAVE. It helps people choose the best rate at the security of the system which whitelists made proposals by community 
- audit & maximize money for them (acquire projects)
### yCRV token
yToken is a certificate of interest and assets. YFI and curve have made more attempts in stable coin(yCRV). yToken is a combination of a set of tokens (DAI, USDC, USDT, and TUSD). The ratio of each token in the combination is variable but they are all stable coins so they can guarantee token exchange within a certain ratio. 
### Features & Advantages of Yearn Finance
- lending aggregator, yield generator service that maximizes interest and minimizes gas consumption.
- governed by the community, realizing the autonomy of DAO.
- YFI & Curve have made some innovations in stable coins, providing more possibilities in stable coin exchange and farming.

## Algorithmic StableCoins 
It has a target price and it won't always stay on the target price. Most projects fail at 1st six months. 

Algorithm stable coins use algorithms to balance the circulating supply of the asset. The algorithm issues more coins when the price increases, and buys them off the market when the price falls. Other types of stable coins are those fiats-collateralized e.g USDT and USDC, crypto-collateralized like DAI which is backed by one or several digital assets like ETH. More often the stability of algorithmic stable coins is compared to those of the two other types of stable coins in terms of trading volumes and market share.

### Rebase Mechanism
> Rebase mechanism takes reference from *Hayek Money: The Cryptocurrency Price Stability* by *Fernando M. Ametrano - 2014, a professor at the Politecnico di Milano, who was once the project leader of the Bitcoin Developers Conference*: “Money supply varies from time to time & key elements of changes are based on requirements by society” and *Robert Sams, a cryptocurrency economist with 11 years of experience in hedge funds*: *Cryptocurrency Stabilization: Seignorage shares*

Top algorithm stable coins in terms of market capitalization currently are Empty Set Dollar (ESD) which is pegged to USD and Ampleforth(AMPL) which is pegged to USD directly and daily rebases to stabilize the price. 

AMPL has been in the market for one year and has been maintaining its peg up to $3 by July last year. Last June it launched an incentive program where users who supplied AMPL-ETH liquidity on Uniswap earned Liquidity Provider fees on Uniswap and users were rewarded for not withdrawing their deposits. AMPL has made over 4K transfers compared to over 8K by DAI, 40K by USDC, and 320k by USDT. 

Algorithm stable coins are still new projects in the Defi space and are expected to adjust with time and reach the goals of algorithm stability without collateral or debt supporting it.
### TWAP price
TWAP(Time Weighted Average Price)
TWAP using Uniswap
```
TWAP = (price cumulative1 - price cumulative2) / (timestamp2 - timestap1)
TWAP = (48120 - 11400)/(1583535828 - 1583532228) = 10.2
```
### First Rebase Token 
Supply changes daily by rebase. It will directly affect the assets in each account. 

*How Rebase Happened*
If the Last TWAP price more than the target price * 1.05. The number of AMPL in all accounts will increase. In 2019 USD multiply the CDAI(1.027) target price whereas if the last TWAP price is less than the target price *0.95. The number of AMPL in all accounts will decrease. money increases and Ampleforth increases.
### Building Rebase Token
The code of this project is simplified by referring to the Ampleforth code.
- Event: transfer, approval, logRebase
- Function:- initialize/totalSupply/ rebase/ totalSupply/ logRebase/ scaledBalanceOf/ transfer scaledTotalSupply/ transferFrom/ approve
- Variable: initial_fragments_supply

> Thank you for reading through my article and others on the blockchain series. If you want to build an algorithm stable you can check [Amplefoth](https://www.ampleforth.org/) code to understand how algorithm stable coin is built using the whole concept of rebasing mechanism. In the next article, I'll touch on front-end development and how to connect a smart contract with front-end(HTML/CSS/js) using web3js. Leave your comments or suggestions if you like this...
