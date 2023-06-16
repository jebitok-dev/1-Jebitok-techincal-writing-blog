---
title: "Introduction to Smart Contracts: Part 3"
datePublished: Sun Feb 28 2021 02:38:04 GMT+0000 (Coordinated Universal Time)
cuid: ckm9zakia07vmhws15qag1iai
slug: introduction-to-smart-contracts-part-3
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1614888083968/Z7iKusjty.png
tags: functions, solidity, smart-contracts

---

In [part 2](https://jebitok.hashnode.dev/introduction-to-dapps-part-2-cklva9pzf0249vos1bj90chlr), we covered an introduction to dApps i.e blockchain accounts and nodes & networks. In this article, we'll cover an introduction to smart contracts and solidity. 

*Overview* 
- smart contracts
- introduction to Solidity
- sample Solidity code

A smart contract is a program that runs on the Ethereum blockchain. It's a collection of code and data that resides at a specific address on the Ethereum blockchain. Solidity is the developer-friendly language for writing smart contracts. The smart contracts must be compiled before they can be deployed so that EVM can interpret and store the contract. Smart contracts are public on Ethereum/BSC and can be thought of as open APIs. 

Solidity is influenced by C++, Python, Javascript and is statically typed. It supports inheritance, libraries, and complex user-defined types. 

Here is an example of solidity code *Hello World in solidity*:

```
// Specifies the version of Solidity, using semantic versioning.
// Learn more: https://solidity.readthedocs.io/en/v0.5.10/layout-of-source-files.html#pragma
pragma solidity ^0.8.0;
// Defines a contract named `HelloWorld`.
// A contract is a collection of functions and data (its state).
// Once deployed, a contract resides at a specific address on the Ethereum blockchain.
// Learn more: https://solidity.readthedocs.io/en/v0.5.10/structure-of-a-contract.html
contract HelloWorld {
    // Declares a state variable `message` of type `string`.
    // State variables are variables whose values are permanently stored in contract storage.
    // The keyword `public` makes variables accessible from outside a contract
    // and creates a function that other contracts or clients can call to access the value.
    string public message;
    // Similar to many class-based object-oriented languages, a constructor is
    // a special function that is only executed upon contract creation.
    // Constructors are used to initialize the contract's data.
    // Learn more: https://solidity.readthedocs.io/en/v0.5.10/contracts.html#constructors
    constructor(string memory initMessage) public {
        // Accepts a string argument `initMessage` and sets the value
        // into the contract's `message` storage variable).
        message = initMessage;
    }
    // A public function that accepts a string argument
    // and updates the `message` storage variable.
    function update(string memory newMessage) public {
        message = newMessage;
    }

    // A view function that accepts a string argument
    // and return a concat string with input `inMessage` memory varibale and `message` storage variable.
    function concatMessage(string memory inMessage) public view returns (string memory) {
        return string(abi.encodePacked(inMessage, message));
    }
}
```
Solidity smart contract is made up of data i.e contract data is assigned to a:-
- location
- storage(persistent data)
- memory:- values that are stored for the lifetime of a contract function execution.
- environment variables have specific global variables and provide information about the blockchain or current transaction. 

```
contract SimpleStorage {
     UINT storeData;
}
```
Solidity data types include:- *boolean, integer, fixed-point numbers, fixed-size byte arrays, dynamically-sized byte arrays, rational & integer literals, string literals, hexadecimal literals, Enums* among others.

In solidity, a function can either be *internal* i.e internal functions and state functions can only be accessed internally whereas *external* i.e external functions are part of the contract interface i.e they can be called from other contracts via transactions.

Functions can also be *public* i.e can be called internally from within the contract or externally via messages or can be *private* i.e only visible for the contract they are defined in and not in derived contracts.

*Constructor functions* are only executed once when the contract is first deployed.

```
 constructor() public {
        owner = msg.sender;
    }
```
ABI i.e Contract Application Binary Interface which is the standard way to interact with contracts in the Ethereum ecosystem from outside the blockchain and for contract-to-contract interaction.

```
{
"inputs": [],
"name": "message",
"outputs": [{
"internalType": "string",
"name": "",
"type": "string"
}],
"stateMutability": "view",
""type": "function"
}
```
```
{ 
"inputs": [
 {
  "internalType": "string",
  "name": "initMessage",
  "type": "string"
 }
], 
"stateMutability": "nonpayable",
"type": "constructor"
}
```
```
{
"inputs": [ 
{
"internallyType": "string",
"name": "newMessage",
"type": "string"
}
],
"name": "update",
"outputs": [],
"stateMutability": "non-payable"
}
```
### Blockchain Dev Tooling

*ChainIDE*
- [ChainIDE for Ethereum](https://eth.chainide.com/)
- [ChainIDE for BSC](https://bscide.com/)
- [ChainIDE homepage](https://chainide.com/)

> Blockchain Developer => Blockchain Solutions => Blockchain Research

- White Matric Integrations


Thank you for reading my article, you can share your thoughts about solidity and smart contracts. In the next article, we'll cover how to write, compile and deploy simple Smart Contract on ChainIDE using Metamask & BSC's Testnet.