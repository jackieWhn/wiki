---
title: Part 2 - Permissioning contract
---

In this part, we will deploy a permissioning contract that will be used for the Secret Store to know which account are allowed to access the decryption keys. It is very similar to the [Part 4 of the Secret Store tutorial](https://wiki.parity.io/Secret-Store-Tutorial-4.html#3-create-and-deploy-a-permissioning-contract).

In fact, the only difference is the code of the contract.

## 1. Deploying a permissioning contract

In the rest of the tutorial, we will use the following  permissioning contract, that allows Bob or Alice to access the decryption key from the secret store.
Here is the contract code that we will use: 
```solidity
pragma solidity ^0.4.11;

contract SSPermissions {
  address alice = 0xe5a4b6f39b4c3e7203ca8caeecbad58d8f29b046;
  address bob = 0xfeacd0d28fd158ba2d3adb6d69d20c723214edc9;

  /// Both Alice and Bob can access the specified document
  function checkPermissions(address user, bytes32 document) constant returns (bool) {
    if (user == alice || user == bob) return true;
    return false;
  }
}
```

This contract is voluntarly open in regards to the `document` id, as we do not know it. Therefore, Bob and Alice are given access to the decryption key, whatever document is requested.

To deploy the contract head to [http://remix.ethereum.org](http://remix.ethereum.org).

- In Remix, create a new file by clicking the `+` at the top left.
- Name it as you wish and paste the code of the contract.
- Make sure to edit it to use your own addresses.
- On the right side select the tab `Run`.
- Make sure you have a node running with `alice.toml`.
- Select in the Environment drop-down menu `Web3 provider`.
- Use the default "http://localhost:8545" as it is the port selected in `alice.toml`.
- Select Alice's account in the next drop-down.
- Click Deploy.

![system overview](images/private-transactions-screenshot-0.jpg)

- On the right side, you will be given the address of the contract that you can copy.
- You can also test your contract right away, in the screenshot above, I verified that Alice can access the keys.

## 4. Modify the Secret Store configuration files

Now that we have the contract deployed on the dev chain, we can specify it in the Secret Store nodes.
Add the `acl_contract` parameter of the `[secretstore]` section of each Secret Store node (`ss1.toml`, `ss2.toml`, `ss3.toml`) as follow:
`acl_contract = "74c369bf541131062ad208576e40aa258ff45d41"` instead of "none"
The full configuration files are accessible [here](https://github.com/Tbaut/Secret-Store-Tutorial-files/blob/master/permissioning-contract/).

## 2. Update the Secret Store nodes configurations

Address: `0x7ac9f71b22cc080bb71bf47b402d6ae71d8c2c0c`
Add `acl_contract = "7ac9f71b22cc080bb71bf47b402d6ae71d8c2c0c"` to ss123.toml without "0x"


|[ ← Part 1 - Configuring each node → ](Private-Transactions-Tutorial-1.md)| [Part 3 -  → ](Private-Transactions-Tutorial-3.md)|
