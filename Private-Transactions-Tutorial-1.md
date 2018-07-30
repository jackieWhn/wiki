---
title: Part 1 - Configuring each node
---

The Secret Store nodes have been configured in the [Secret Store tutorial](https://wiki.parity.io/Secret-Store-Tutorial-overview) and do not need to be modified for now.
Unlike the Secret Store tutorial, we will setup 2 distinct nodes for Alice and Bob as they will have distinct roles.
Alice's address will be used as the external account to sign and interract with the private contract, while Bob will be a validator (decoding Alice's transactions).

### 1. Configure Alice's node
We will create a config file named `alice.toml` with the based on `users.toml` from the [Secret Store Tutorial](https://wiki.parity.io/Secret-Store-Tutorial-overview).
Alice's node needs its dedicated data directory. Make sure you add the relevant JSON-RPC APIs to Alice's node: `["secretstore","eth","net","private","parity","personal"]` as well.
The list of bootnodes correspond to the Secret Store nodes for now, we will add Alice and Bob's node in a next step.

Here is how the `alice.toml` file look like for now.

```toml
# Alice's config file in alice.toml

[parity]
chain = "dev"
base_path = "db.alice" #use a dedicated data directory

[rpc]
port = 8545 #default http port for RPC
apis = ["secretstore","eth","net","private","parity","personal"]    #add "private","parity","personal"
cors = ["http://remix.ethereum.org"]                                # allow remix to access this node

[secretstore]
disable = true # users do not run a secret store node

[network]
port = 30300
bootnodes = [
  "enode://59ff7f71a8ced85f23d2455b8645931e962886cce7025fd4fb769c4881c505d8445aa24be98b1aa3067cf7490a2ff0cd1558c37f6a536a4d799f8d93c3fe21ea@127.0.0.1:30301",
  "enode://800cf3b097974b9740d9d7aeda28bf1655e7c30a10bdb5f616ee0b41b786c13ce8d4008854d96430193b7cb4710a59c418566d5f6111bce4a18319757eaec358@127.0.0.1:30302",
	"enode://58815b57d8af2bc04963bde42b27deca674c18dca4098b8891296479ce0a83c2398a141babb835f181c6447bb1ac2ce4dca88ec20908d41b86166018d842fab4@127.0.0.1:30303",
]

```

### 2. Configure Bob's node

Bob's configuration file is even simpler as it will only be used as a validator. No need for RPC server here.
We will create a config file named `alice.toml`, using a dedicated directory for Bob and a network port that isn't used yet.

Bob's toml
```bash
# Users config file in bob.toml

[parity]
chain = "dev"
base_path = "db.bob" #use a dedicated data directory

[rpc]
disable = true

[websockets]
disable = true

[secretstore]
disable = true # users do not run a secret store node

[network]
port = 30304  #use a dedicated networking port
bootnodes = [
  "enode://59ff7f71a8ced85f23d2455b8645931e962886cce7025fd4fb769c4881c505d8445aa24be98b1aa3067cf7490a2ff0cd1558c37f6a536a4d799f8d93c3fe21ea@127.0.0.1:30301",
  "enode://800cf3b097974b9740d9d7aeda28bf1655e7c30a10bdb5f616ee0b41b786c13ce8d4008854d96430193b7cb4710a59c418566d5f6111bce4a18319757eaec358@127.0.0.1:30302",
	"enode://58815b57d8af2bc04963bde42b27deca674c18dca4098b8891296479ce0a83c2398a141babb835f181c6447bb1ac2ce4dca88ec20908d41b86166018d842fab4@127.0.0.1:30303",
]
```

## 3. Finalize configuration
 
### 3.1 Collect enodes's addresses
 
We will need to unlock Alice and Bob's account, and make sure that their nodes are connected together. To do so, we first need to know the enode. Launching parity with an incomplete toml configuration file still allows us to read the enode address and let Parity Ethereum  creates the needed directories for us to copy the users' keys in.

Launch Parity Ethereum with `parity --config alice.toml` and copy the enode address printed in the console. Do the same for Bob with `parity --config bob.toml`.
for instance:
Alice: `enode://29ebcddebe4d2c13632080e12a39ef89ff245893e3a7685676a01cbea011395bfce72e82664a9379eb5f3916e9e4f0df5de65d2559e87c28ceb291d53aee6fe0@127.0.0.1:30300`
Bob: `enode://00db9262e364d683852488c0b404b146268bc1be3df01bbd572e33692d4a8dbd4b8b6ab300e70a01cb3a5024fc164fc355041afdfe82642f3e36c61763b0f22d@127.0.0.1:30304`

Note that you should replace the IP at the end to use localhost `127.0.0.1`.
We now have two new directories (db.alice and db.bob) where we will copy the keys from Alice and Bob created in the Secret Store tutorial.

### 3.2 Manage Alice and Bob's keys

If you wish to use the same users and keys as the Secret Store tutorial, go ahead and copy Alice and Bob's keys from the `db.users/keys/DevelopmentChain/` directory into `db.alice/keys/DevelopmentChain/` and `db.bob/keys/DevelopmentChain/` respectively. Using the following command for instance:

```bash
cp Secret_store_tutorial_directory/db.users/keys/DevelopmentChain/file-for-alice-key Private_tx_tutorial_directory/db.alice/keys/DevelopmentChain/file-for-alice-key

```

To know which file correspond to which key, simply open them with a text editor and read the addresses.

You should also create password files for Alice and Bob, `alice.pwd` and `bob.pwd` with `alicepwd` and `bobpwd` respectively. You can do so using the command:
```bash

echo alicepwd > alice.pwd
echo bobpwd > bob.pwd
```

### 3.3 Update Alice and Bob's configuration files

You can now edit Alice and Bob's configuration files to add the bootnodes to each file, and unlock the accounts:

Both configuration files should now have the following bootnodes:
```toml
bootnodes = [
  "enode://59ff7f71a8ced85f23d2455b8645931e962886cce7025fd4fb769c4881c505d8445aa24be98b1aa3067cf7490a2ff0cd1558c37f6a536a4d799f8d93c3fe21ea@127.0.0.1:30301",
  "enode://800cf3b097974b9740d9d7aeda28bf1655e7c30a10bdb5f616ee0b41b786c13ce8d4008854d96430193b7cb4710a59c418566d5f6111bce4a18319757eaec358@127.0.0.1:30302",
	"enode://58815b57d8af2bc04963bde42b27deca674c18dca4098b8891296479ce0a83c2398a141babb835f181c6447bb1ac2ce4dca88ec20908d41b86166018d842fab4@127.0.0.1:30303",
  "enode://29ebcddebe4d2c13632080e12a39ef89ff245893e3a7685676a01cbea011395bfce72e82664a9379eb5f3916e9e4f0df5de65d2559e87c28ceb291d53aee6fe0@127.0.0.1:30300",
  "enode://00db9262e364d683852488c0b404b146268bc1be3df01bbd572e33692d4a8dbd4b8b6ab300e70a01cb3a5024fc164fc355041afdfe82642f3e36c61763b0f22d@127.0.0.1:30304"
]
```

Add the following to `alice.toml`:
```toml
[account] # unlock Alice's account to deploy the contract
unlock = ["0xe5a4b6f39b4c3e7203ca8caeecbad58d8f29b046"]
password = ["alice.pwd"]
```

Add the following to `bob.toml`:
```toml
[account] # unlock Bob's account to deploy the contract
unlock = ["0xfeacd0d28fd158ba2d3adb6d69d20c723214edc9"]
password = ["bob.pwd"]
```

You can find the complere set of files in [this repository](https://github.com/Tbaut/Private-Transations-Tutorial-files/tree/master/config).

## 4. launch the nodes with the correct flags

Alice and Bob's nodes are now configured, we can launch them with the dedicated private flags:

Alice's node should be launched with:

- `--private-tx-enabled` to allow private transaction feature.
- `--private-signer="0x_Alice_s_address"` to sign public transaction with Alice's key
- `--private-account="0x_Alice_s_address"` to sign private transaction (to the Secret Store) with Alice's key
- `--private-sstore-url="http://127.0.0.1:8010"` to indicated what address to use to reach the secret store
- `--private-passwords alice.pwd` to unlock Alice's account to sign any transaction (private and public)

Alice's node can be launched with:
`parity --config alice.toml --private-tx-enabled --private-signer="0xe5a4b6f39b4c3e7203ca8caeecbad58d8f29b046" --private-account="0xe5a4b6f39b4c3e7203ca8caeecbad58d8f29b046" --private-sstore-url="http://127.0.0.1:8010" --private-passwords alice.pwd`

For Bob, as a validator, we will use:
- `--private-tx-enabled` to allow private transaction feature.
- `--private-validators="0x_Bob_s_address"` to specify that Bob is a validator.
- `--private-account="0x_Bob_s_address"` to sign private transaction (to the Secret Store) with Alice's key.
- `--private-sstore-url="http://127.0.0.1:8010"` to indicated what address to use to reach the secret store.
- `--private-passwords bob.pwd` to unlock Bob's account to sign private transactions.

Bob's node can be launched with:
`parity --config bob.toml --private-tx-enabled --private-validators="0xfeacd0d28fd158ba2d3adb6d69d20c723214edc9" --private-account="0xfeacd0d28fd158ba2d3adb6d69d20c723214edc9" --private-sstore-url="http://127.0.0.1:8010" --private-passwords bob.pwd`

|[ ← Tutorial overview](Private-Transactions-Tutorial-Overview.md) | [Part 2 - Permissioning contract → ](Private-Transactions-Tutorial-2.md)|
