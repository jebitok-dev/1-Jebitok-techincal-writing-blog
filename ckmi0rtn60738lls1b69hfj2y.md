---
title: "BSC Smart Contract 101 : How to write and deploy simple storage contract in BSC"
datePublished: Sat Mar 20 2021 17:43:02 GMT+0000 (Coordinated Universal Time)
cuid: ckmi0rtn60738lls1b69hfj2y
slug: bsc-smart-contract-101-how-to-write-and-deploy-simple-storage-contract-in-bsc
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1616262086604/RgPOTwUI4.png
tags: ides, solidity, smart-contracts

---

Select Get started(English) V.1.0.0 template on [ChainIDE homepage](https://bscide.com/project/welcome) this a tutorial on how to deploy a simple Decentralized Finance Smart Contract project. There's also another Binance MasterClass Week3 Template that has tutorial and assignment to help you if you're an absolute beginner it has sample code which you can compile and deploy & interact with the smart contracts on the chainIDE before writing your own.

Binance Smart Contract(BSC) runs smart contracts & supports EVM(Ethereum Virtual Machine).  It uses BNB mortgage to participate in successful verification of valid blocks given proposed transaction fees in the blocks which can be changed. 
### how to write simple smart contract using solidity on ChainIDE

- create a new project on the IDE
Two projects will be automatically created i.e type reference of ERC20 
- create a .sol file that enables users to deposit and withdraw from the smart contract. 
The contract has variables and two functions corresponding to writing & reading respecting. 

```
pragma solidity ^ 0.6.2;

contract AddressBalance {
    mapping(address => uint ) public balances;
    function setCurrentBalance(uint currentBalance) public {
        balances[msg.sender] = currentBalance;
    }
}

contract Update {
    function setCurrentBalance() public returns (uint) {
        AddressBalance addressBalance = new AddressBalance();
        addressBalance.setCurrentBalance(10);
        return addressBalance.balances(address(this));
    }
}
```

- compile the .sol file after writing the smart contract. (Compile button on the top right side on the sidebar of the IDE)
Check the compiler version to suit the one on pragma solidity as defined on the contract then click compile. In case of errors, the output will be shown at the terminal of IDE correct them and compile again until you get 
````
compile contract 
compile contract success
````
The next step is to deploy the smart contract but before that, we need the network setup with Metamask to fund the wallet with BNB:-

### how to set up the Testnet and Metamask wallet 

The smart contract deployed on the BSC network can use the
- *test network(Testnet)* 
- *main network (Mainnet)*. 
> Mainnet you have to use BNB purchased from the Binance Marketplace and deposit it in its own BEP20 address before deployment

We'll focus on the *Testnet*, here deployment cost is low and takes a simple process. Deployment on BSC requires some setting for Metamask:-
- open small fox wallet on Metamask(Add Metamask extension on your browser)
- select Ethereum Mainnet(network option) then click on Custom RPC and fill the 5places 
````
Network Name: BSC Testnet
New RPC URL: https://data-seed-prebsc-1-SI-binance.org.8545/
Chain ID: 97
Currency Symbol: BNB
Block ExplorerURN: https://testnet.bscscan.com
````
Click & save then the BSC Testnet option appears in the network option bar. Go to the faucet of [ the BSC test network](https://testnet.binance.org/faucet-smart/) to get tokens. Enter your wallet address(copy from the middle of the open Metamask window). Switch network to BSC Testnet and you will see some BNB on your wallet.

connect Metamask to your ChainIDE website by switching the browser to ChainIDE window then click on the Metamask icon where 3dots will appear on the upper-right corner then select connect site. A box will appear click it manually to connect to the current site and click to confirm step by step.(there will be a green dot to show that it has connected on Metamask)


### how to deploy  a smart contract on BSC ChainIDE & Metamask

 click deploy & interaction on the ChainIDE sidebar. Click deploy then set value to 1,000,000 and confirm the contract to be deployed is correct then click deploy. Jump out of Metamask interface if gas is above the set value modify it. If the deployment fails it means the gas price is high. On successful deploy info-interface and functions in the contract will appear o the right with deployment successful. 

Click and interact with the  smart contract by submitting values of function to implement the contract.