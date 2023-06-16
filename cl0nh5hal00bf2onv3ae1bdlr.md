---
title: "#100DaysofCode: GETH /Connecting Testnet & Metamask"
datePublished: Sat Mar 12 2022 06:36:17 GMT+0000 (Coordinated Universal Time)
cuid: cl0nh5hal00bf2onv3ae1bdlr
slug: 100daysofcode-geth-connecting-testnet-and-metamask
canonical: https://sharonjebitok.notion.site/Installing-GETH-Connecting-Testnet-Metamask-cc38870d54814f1db08b9df42eaf1dba
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1647066870788/R0906xHQt.png
tags: go, test, blockchain, ethereum

---

## GETH
- [Go Ethereum](https://geth.ethereum.org/) is one of the three original implementations (along with C++ and Python) of the Ethereum protocol. It is written in Go, fully open-source, and licensed under the GNU LGPL v3.
- Go Ethereum is available either as a standalone client called Geth that you can [install](https://geth.ethereum.org/docs/install-and-build/installing-geth) on pretty much any operating system or as a library that you can embed in your Go, Android, or iOS projects.

## Connecting to Testnet
[Add Metamask to your browser extension](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=en)

Add Testnet networks to your Metamask that will be helpful in deploying your smart contracts
- [Rinkeby](https://faucets.chain.link/rinkeby): click on the link to get many test-nets
- [Kovan](https://faucets.chain.link/kovan)
- Mumbai: [Mumbai testnet faucets](https://faucets.chain.link/mumbai) or [faucet](https://faucet.polygon.technology/)
```
Network Name: Mumbai
New RPC URL: [https://matic-mumbai.chainstacklabs.com/](https://matic-mumbai.chainstacklabs.com/)
Chain ID: 80001
Currency Symbol(Optional): Matic-test
```
- [Connecting Binance Smart Chain to Metamask](https://academy.binance.com/en/articles/connecting-metamask-to-binance-smart-chain)
```
Network Name: Smart Chain - Testnet
New RPC URL: https://data-seed-prebsc-1-s1.binance.org:8545/
ChainID: 97
Symbol: BNB
Block Explorer URL: https://testnet.bscscan.com
```

### Testnet Faucet links

- [MoonBorrow](https://moonborrow.com/)
- [Rinkeby](https://faucets.chain.link/chapel)
- [Faucet Ropsten](https://faucet.ropsten.be/)

You can connect Metamask to the main net.

Remix: write a contract and compile then deploy and select injected web3 option to connect Metamask

## Network Scans
### [EtherScan](https://etherscan.io/)

- [EtherScan](https://etherscan.io/) is a block explorer and analytics platform that allows you to access details on any Ethereum blockchain transactions that are pending or confirmed.
- It is the most trusted tool for navigating through all the public data on the Ethereum blockchain and is sometimes called “Ethplorer.” This data includes transaction data, wallet addresses, smart contracts, and much more.

Users can use Etherscan to:

- Calculate Ethereum gas fees with the Etherscan gas tracker
Lookup and verify smart contracts
- View the crypto assets held in or associated with a public wallet address
- Observe live transactions taking place on the Ethereum blockchain
- Lookup a single transaction made from any Ethereum wallet
- Discover which smart contracts have a verified source code and security audit
- Keep track of how many smart contracts a user has authorized with their wallet
- Review and revoke access to a wallet for any decentralized applications (DApps)

### [polygon](https://polygonscan.com)
- PolygonScan allows you to explore and search the Polygon blockchain for transactions, addresses, tokens, prices, and other activities taking place on Polygon
###  [BSC Scan](https://bscscan.com/)
- BscScan. is the leading blockchain explorer, search, API, and analytics platform for BNB Smart Chain
###  [Rinkeby EtherScan](https://rinkeby.etherscan.io/)
- Etherscan allows you to explore and search the Rinkeby blockchain for transactions, addresses, tokens, prices, and other activities taking place on Rinkeby
###  [Mumbai Polygon Scan](https://mumbai.polygonscan.com/)
- PolygonScan allows you to explore and search the Mumbai blockchain for transactions, addresses, tokens, prices, and other activities taking place on Polygon
###  [Kovan](https://kovan.etherscan.io/)
- Etherscan allows you to explore and search the Kovan blockchain for transactions, addresses, tokens, prices, and other activities taking place on Kovan (KETH)

Thank you for reading through my article on this series. I will be dropping more articles linked to my **100DaysofWeb3** journey. You can leave a comment or suggestion. We can also connect more on [Twitter](Twitter.com/SharonJebitok) or [LinkedIn](LinkedIn.com/in/sharon-jebitok).