---
title: "Solidity & Smart Contract Development"
datePublished: Sun Feb 20 2022 20:46:06 GMT+0000 (Coordinated Universal Time)
cuid: ckzvqpc7h02s2dhs10m0l0d64
slug: solidity-and-smart-contract-development
canonical: https://www.notion.so/sharonjebitok/smart-contracts-solidity-development-7d0c6d4ae2b644e5ae5c40042b2f04dc
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1645389954847/LTjnsQkRh.png
tags: programming-blogs, blockchain, solidity, smart-contracts

---

This article covers smart contracts and solidity development which is a continuation of my [previous article](https://jebitok.hashnode.dev/blockchain-development-tools-ckyq32oy70atpans10nck4muu).

## Solidity
Solidity is the programming language for writing smart contracts on the Ethereum blockchain. The [solidity documentation](https://docs.soliditylang.org/en/v0.8.11/) helps you write and understand smart contracts. The syntax of solidity is almost like JavaScript. 
### Difference between Solidity and JavaScript
Solidity is:
- hyper language i.e have to declare the type of all variables. 
- compile language i.e after writing code you have to go through phases of compilation which will transform how it's high-level to low-level instructions that EVM will be able to understand. Unlike Javascript that you write code and run it on a browser or your devices. 
- it's limited unlike Javascript allows string manipulation which is not possible with solidity.
- it has a few lines of code 
- can only run on the blockchain, unlike Javascript which runs on the browser and other environments.
[Vyper](https://vyper.readthedocs.io/en/stable/) is the other programming language used to write smart contracts on Ethereum blockchain but solidity is common and supported by the [Truffle](https://trufflesuite.com/) framework.

[Remix](https://remix.ethereum.org/) has autocompile feature 
## Structure of solidity smart contract structure
### SPDX license identifiers

Solidity from version 0.6.8 introduced the introduced SPDX license identifiers so developers can specify the [license 1.4k](https://spdx.org/licenses/) the contract uses. (e.g. OpenZeppelin Contracts use MIT license).

```solidity
// SPDX-License-Identifier: MIT
```

## Pragma Statement

made of pragma keyword, solidity, caret, plus version number, and semicolon. This ensures we use the correct version of solidity. Using caret means we can compile using any version of solidity equal to that version number specified or higher than it.

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.6.0;

contract MyContract {

}
```

## Variable Types

Contrary to Javascript, in solidity, you have to declare variables before you use them.

### Fixed-size types:

It occupies fixed size in memory

*Examples:*

- Boolean type:

```solidity
  // isReady: true or false;
```

- unit 0: important when sending transaction of Ether or ERC20 token. It’s like number type in Javascript except that in solidity it can only hold positive numbers which are integers and not floating-point numbers

```solidity
uint 0;
```

- address: recipient address and address of a smart contract

```solidity
address recipient;
```

- Byte32 data: can hold any arbitrary binary data. It’s used to represent the string as with solidity it's hard to manipulate strings.  Byte32 is used to represent strings whose size doesn’t exceed 32bytes

### Variable size types:

bit complex and can hold variable size in terms of length

- *string*: a string of any length and unlike Javascript we don’t have any convenience function to manipulate strings. In many cases, people prefer to represent their string as byte32 and in other cases represented using bytes which is a generalization of bytes32 and used to represent binary data of no predefined length

```solidity
string name;
bytes _data; //bytes
```

- *arrays:* used to represent a collection of data. Contrary to Javascript in solidity arrays have to be arrays of the same type e.g array of integers, an array of byte32

```solidity
uint[] amounts;
```

- *mappings:* associative array they have keys that map into value. define the type of key and type of value mapped by each key and define the type of mappings. It's similar to Javascript objects and how they work.

```solidity
mapping(uint => string) users;
```

### user-defined data:

custom representation of data as commonly used in C language.

- *struct:* you use struct keyword then name it in uppercase and you’ll define different fields. It’s similar to a Javascript object but here you can’t instantiate a container. Struct types are used a lot in solidity

```solidity
struct User {
		uint id;
		string name;
		uint[] friendIds;
}
```

- *Enum:* defined using enum keyword and after you name your struct. define different options. Used as sort of labels

```solidity
enum Color {
	RED,
	GREEN,
	BLUE,
}
```

Built-in Variables

```solidity
msg.sender;
```

this is the address of the sender of the transaction. It’s impossible to fake that due to the cryptography of Ethereum you’re sure that ```msg.sender``` is populated into the address of the sender.

```solidity
msg.value
```

Value of Ether that was sent in a transaction. 

```solidity
// Unix
now block.timestamp
```

[Timestamp](https://www.unixtimestamp.com/) in seconds i.e time elapsed since January 1st, 1970. Unix could reference the timestamp of a block from the time it was mined and it can be manipulated by miners.  

You can check out [other built variables here](https://docs.soliditylang.org/en/v0.5.3/units-and-global-variables.html#block-and-transaction-properties) on solidity docs

### Constructor

function, where you can run some initialization code inside a contract the code, is only run once when a smart contract is deployed. Some constructors can be defined with function keyword this was valid in lower versions of solidity.

```solidity
constructor(uint _a) public {
		a = _a;
}
```

### Declaring Functions

Functions are helpful in manipulating data. 

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

constructor MyContract {
	uint value;
	// view - read only
	function getValue() external view returns(uint) {
		return value;
	}

	function setValue(uint _value) external {
			value = _value; // assigning variable
	}
 
}
```

### Function Visibility

Function visibility keywords: they define who can have access to your function

- Restrictive visibility keyword:
    - *private:* function can only be called within the smart contract. The function is prefixed with private (just good practice not a must)
    - *internal*: function is limited to be called within a smart contract and from a smart contract that inherits from the smart contract. Solidity allows inheritance mechanisms like in the C language.
    - *External:* function is only called from the outside smart contract. Isn’t prefixed with _private.
    - Public: can be called within and outside the smart contract. It is the most permissive function visibility keyword.
    
    The good rule of thumb for smart contracts is to give the least privilege to any entity first try private then internal, external, or lastly public. The greatest beginner mistake is using Public for all contracts.
    
    ### Variable Visibility
    
    declares integer variable with private and name i.e can be read within the same smart contract. 
    
    ```solidity
    // SPDX-License-Identifier: MIT
    
    pragma solidity ^0.8.0;
    
    contract MyContract {
    	uint private a;
    
    	function foo() external {
    			uint b = a + 1;
    	}
    }
    ```
    
    - a *private* variable is readable within the foo function. The level of restriction of private keywords is only valid with the EVM which is within Ethereum Blockchain which is a public blockchain that doesn’t store anything private as people will be able to access it on other platforms.
    - *Internal*: can be read within smart contract and smart contract that inherits it but can’t be read in another smart contract.
    - *public*: can be read from within and outside the smart contract. It’s not a must to declare a public keyword as solidity does it on default.
    
    Use least possible privilege, try private then internal if doesn’t work go for the public keyword.
    
    ```solidity
    // SPDX-License-Identifier: MIT
    
    pragma solidity ^0.8.0;
    
    contract MyContract {
    	uint public a;
    	// you'll get an error since solidity has defined variable globally
    	/*function foo() public view returns {
    			return = a;
    	} */
    
    	function foo() external {
    			uint b = a + 1;
    	}
    }
    ```
    
    ### Deploying & interacting with a smart contract in Remix
    
    write a contract with on [remix](https://remix.ethereum.org/) editor, with pragma statement and the License from Open-Zeplin. Then compile using Remix’s auto compile feature and then deploy on Remix.
    
    ### write a contract with on remix editor, with pragma statement and the License from Open-Zeplin. Then compile using Remix’s auto compile feature and then deploy on Remix.
    
    - *if/ if else:* function is only executed if conditions are met
    
    ```solidity
    // SPDX license identifiers
    
    pragma solidity ^0.8.0;
    
    contract MyContract {
    	function foo() external {
    		// doesn't use superior equal sign(===) like javascript only uses (==)
    		if(msg.sender >= 100 && block.timestamp > 12345677... ||  ) {
    
    		}
    		// function is only executed if conditions are met
    		if(boolValue) {
    		
    		} else {
    
    		}
    	}
    }
    ```
    
- for loop

```solidity
 // SPDX license identifiers

pragma solidity ^0.8.0;

contract MyContract {
	bool boolValue;
	function foo() external {
		for(uint i = 0; i < 10; i++) {
				
		}
	}
}
```

- while loop: same as for loop, it allows you to go through a set of data or compilation at a certain amount of time. Can’t know how long you’ll need to run the code i.e infinite loop. Can use the *break* and *continue* conditions.

```solidity
// SPDX license identifiers

pragma solidity ^0.8.0;

contract MyContract {
	function foo() external {
		// testing condition which must be true or false
		bool isOk = true;
		while (isOk) {
				//
				if() {
					isOk: false;
					// break;
					// continue
			}
		}
	}
}
```

### Arrays

declare and manipulate the array. Arrays allow us to represent the compilation of data. It must contain an array of the same type i.e integer 

*Storage Arrays*

- Fixed-size array: specify the size of the array and lose control of the push method
- Dynamic size Array(don’t specify the size of the array just add using push method)

*Memory Arrays*

- has to be declared. They are not safe in a contract since you can’t save data on them

* Accept array arguments and return arrays from data*

external takes call data, public & internal takes memory
```
// SPDX license identifiers

pragma solidity ^0.8.0;

contract MyContract {
	uint[] myArray; 	//CRUD - CREATE/READ/UPDATE/DELETE

	// Dynamic array
	function foo() external {
		// adding elements to array
		myArray.push(2);
		myArray.push(3);
		// reading elements to array
		myArray[0];
		//updating elements to array
		myArray[0] = 20;
		//myArray[0];
		// delete element to array
		delete myArray[1];
			//myArray[1] = 0;
		// iterating through array
		for(uint i = 0; i <= myArray.length; i++) {
			myArray[i]
		}
	}
	// Memory array
	function foo() external {
		uint[] memory newArray = new uint[](10);
		newArray[0] = 10;
	
		newArray[0];
		newArray[0] = 20;
		delete newArray[2];
	}
	// Accept & Reference Array in function
	function foooBar(uint[] memory myArg) internal returns(uint[] memory ){
			
	}
}
```
### Mappings

Declare, manipulate, CRUD elements, default, nesting mappings. Has keys & values like objects in Javascript. It’s possible to iterate through all entries of mappings so in solidity you combine arrays and mappings to represent collections of data.
```
// SPDX license identifiers

pragma solidity ^0.8.0;

contract MyContract {
	mapping(address => uint) balances;
	// owner of address and address of another approved to use initial address(another person's address)
	mapping(address => mapping(address => bool)) approved;
		mapping(address => uint[]) scores;
	function foo() external {
		// Add
		balances[msg.sender] = 100;
			// Read
		balances[msg.sender];
			// Update
		balances[msg.sender] = 200;
			// delete
		delete balances[msg.sender];
			// default value
		balances[someAddressThatDoNotExist] => 0
			// exotic mapping 1
		approved[msg.sender][spender] = true;
		approved[msg.sender][spender];
		approved[msg.sender][spender] = false;
		delete approved[msg.sender][spender];

		// exotic mapping 2
		scores[msg.sender].push(1);
		scores[msg.sender].push(2);
		scores[msg.sender][0];
		scores[msg.sender] [0] = 10;
		delete scores[msg.sender][0];
	}
}
```

### Structs

- structs are good to represent structured data in the smart contract. Define a template with different fields that represent your data then create several clones of the templates with the same field but different values. It’s the same with classes in Javascript except that in solidity struct can only define a field, not methods.
- If you find yourself defining a lot of variables on your smart contract it is a sign that you should be using a struct.

```solidity
// SPDX license identifiers

pragma solidity ^0.8.0;

contract MyContract {

	struct User { //template
		address addr;
		uint score;
		string name;
	}
	// declare array of struct
	User[] users;
	// define mapping
	mapping(address => User) userList2;

	function foo() external { //parameter
		User memory user1 = User(msg.sender, 0, _name);
		User memory user2 = User(msg.sender, 0, _name);
		User memory user3 = User(name: _name, score: 0, addr: msg,sender);
		// no particular order but its verbalsome have to choose the notation
		user3.addr;
		user3.score = 20;
		delete user1;

		users.push(user2);
		userList2[msg.sender] = User();
	}
}
```

### Events

- With solidity events, you can push data from smart contracts to the web frontend or mobile application. For example, if it's a Defi project you can create events on the smart contract so that whenever traders make transactions on the front-end the contract emits events that can inform traders of ongoing trades. There could be a lot of events in a contract and you need to use index keywords to filter specific events. You can use a maximum of 3 indexes on a contract.
- An event can’t be read within the contract once consumed it’s consumed by entities outside the contract. Instead of a storage variable, you can use an event
- Two steps: declare event on the contract level and on the front-end side of a dApp.

```solidity
// SPDX license identifiers

pragma solidity ^0.8.0;

contract MyContract {
	event NewTrade (
		uint date,
		address index from,
		address to;
		uint amount;
	)
	function trade(address to, uint amount) external {
		emit NewTrade(now, msg.sender, to, amount);	
	}
}
```

### Interacting between smart contracts

- calling function of another contract. Syntax, import keyword, contract interfaces(minimum information required for the contract to interact with another), how propagation works. (Deploy on Remix to see how the two contracts interact) e.g ERC20 token with IERC20

```solidity
// SPDX license identifiers

pragma solidity ^0.8.0;

import "ContractB"

contract A {
	// interface of B/refernce B & address of B
	address addressB;

	function setAddressB(address _addressB) external {
		address = _addressB;
	}
	function callHelloWorld() external {
		Interface b = Interfaec(address);
		// B b - B(addressB);
	returns B helloWorld();
	}
}

contract B {
	function helloWorld() external pure returns(string memory) {
		return "Hello world!";
	}
}
```

```solidity
contractb.sol
// SPDX license identifiers

pragma solidity ^0.8.0;
// contract interface - will be called in another contract
 contract Interface {
		function helloWorld() external pure returns(string memory);
		// function foo() external;
}
contract B {
	function helloWorld() external pure returns(string memory) {
		return "Hello world!";
	}
	
	function foo() external {
		
	}
```

Transferring Ether

[Ether units in solidity](https://docs.soliditylang.org/en/v0.6.4/units-and-global-variables.html#ether-units)

manipulating Ether on solidity smart contract is something powerful that can be done. Address controlled by private key

```solidity
// SPDX-License-Identifier: MIT;

pragma solidity 0.8.0;

contract MyContract {
    function sendEther(address payable to, uint amount) external {
        to.transfer(amount);
        // to.transfer(100); 
        // 1 wei = 10 ^ -18 ether = 0.000....1
    } // address payable/ sendable address

    function receiveEther() external payable {
        msg.value; // amount of ether balance
        address(this).balance;
    }

    fallback() external {
        //
    }

    receive() external payable{
        //
    }
}
```

Dealing with Errors(require, throw)

- errors in solidity smart contract: what happens when there’s an error in solidity function,
- keywords used to create an error like throw, require, assert
- what happens when there are errors in other smart contracts

```solidity
// SPDX-License-Identifier: MIT;

pragma solidity 0.8.4;

contract ErrorContract {
    uint a;
    function throwError() external {
        a = 10;
        // BOOM error!
        // gas will be consumed: avoid errors: trigger them if one of your arguemnets of function are out of range
    }

    function throwErr() external {
        // was the first to be used in solidity and was depricated
        // throw
    }
        
        uint b;
    function revertErr() view external {
        // specify why it was reverted
        if(b == 10) {
            revert("this is why it reverts");
        }
    }

    function requireErr() view external {
        require(a != 10, 'this is why it reverts');
        // errors that can happen in lifecycle method of the contract
    }

    function assertErr() view external {
        assert(a != 10);
        // can never happen in the smart contract
    }

    function willThrow() pure external {
        // revert("because reasons"); //view on deploy
        require(true == false, 'because reasons');
        assert(true == false);
    }

    function foo() external returns(bool) {
        B c = new B();
        address(c).call(abi.encodePacked("bar()"));
        return false;
    }
}

contract B {
    function bar() pure external {
        revert("because of other reasons");
    }
}
```

Custom Function Modifiers

- function modifiers allow you to run some code before you execute some functions. It is good for access control and validating some agreements.
- basic syntax, pass arguments, and change them

```solidity
// SPDX-License-Identifier: MIT;

pragma solidity 0.8.4;

contract FunctionModifers {
    // uint a = 10;
    address admin;
    function basicSyntax(uint a) external myModifier(a) myModifier2(a) {
        // modifier is executed first then placeholder which is the function
    }

    function onlyPerson() external onlyAdmin() {
        
    }

    modifier onlyAdmin() {
        require(msg.sender == admin, "only admin can call this function");
        _;
    }

    modifier myModifier(uint a) { // internal by default
        require(a == 10, "my error message");
        _; // placeholder that stands for function
    }

       modifier myModifier2(uint a) { // internal by default
        require(a == 10, "my error message");
        _; // placeholder that stands for function
    }
    // first modifier then next & foo function
}
```

Inheritance

Inheritance: offers you an opportunity to reuse some code
- [Inheritance trees](https://medium.com/upstate-interactive/inheritance-trees-are-amazingly-helpful-for-solidity-developers-e365e362196f)

courtesy of SolidityByExample
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

/* Graph of inheritance
    A
   / \
  B   C
 / \ /
F  D,E

*/

contract A {
    function foo() public pure virtual returns (string memory) {
        return "A";
    }
}

// Contracts inherit other contracts by using the keyword 'is'.
contract B is A {
    // Override A.foo()
    function foo() public pure virtual override returns (string memory) {
        return "B";
    }
}

contract C is A {
    // Override A.foo()
    function foo() public pure virtual override returns (string memory) {
        return "C";
    }
}

// Contracts can inherit from multiple parent contracts.
// When a function is called that is defined multiple times in
// different contracts, parent contracts are searched from
// right to left, and in depth-first manner.

contract D is B, C {
    // D.foo() returns "C"
    // since C is the right most parent contract with function foo()
    function foo() public pure override(B, C) returns (string memory) {
        return super.foo();
    }
}

contract E is C, B {
    // E.foo() returns "B"
    // since B is the right most parent contract with function foo()
    function foo() public pure override(C, B) returns (string memory) {
        return super.foo();
    }
}

// Inheritance must be ordered from “most base-like” to “most derived”.
// Swapping the order of A and B will throw a compilation error.
contract F is A, B {
    function foo() public pure override(A, B) returns (string memory) {
        return super.foo();
    }
}
```

Thank you for reading through my article despite its length wanted to put together the basics of solidity and smart contracts. Disclaimer most code snippets are more relevant to lower versions of solidity check [Solidty Docs](https://docs.soliditylang.org/en/v0.8.12/), [Solidity by Example](https://solidity-by-example.org/) for more relevant ones for higher versions of solidity.

We connect more on [Twitter](https://twitter.com/SharonJebitok) or [LinkedIn](https://www.linkedin.com/in/sharon-jebitok/). You can leave a comment, suggestion, or point of correction all are welcomed. 