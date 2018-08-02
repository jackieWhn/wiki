---
title: Private Transactions Tutorial Overview
---

In this tutorial, we will learn step by step how to setup a Private contract and make private transactions. The use case is the following:

Alice and Bob would like to use a smart-contract on a blockchain, but want to make sure no-one else can read the state of the contract or interact with it.

To follow this tutorial, you need to have a set of Secret Store nodes running. Follow the [Secret Store tutorial](https://wiki.parity.io/Secret-Store-Tutorial-overview) to get familiar with it. This tutorial will pick-up from there.
 

To keep things simple we will work on a [Private development chain](Private-development-chain) but the setup presented would work on any blockchain, Private or Public.


The overall setup looks as follow:

- 2 regular user accounts (Alice and Bob) 
- 3 Secret Store nodes to manage the contract and transaction's encryption key
- 1 Permissioning contract allowing Bob and Alice to interact with the Secret Store
- 1 Private contract that only Alice and Bob can interact with.


## Table of contents:

|[ Part 1 - Configuring each node → ](Private-Transactions-Tutorial-1.md)|


