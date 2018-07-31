---
title: Part 4 - Send private transactions
---


Call x() : to see the variable 
Per default any public variable such as `x` is viewable, in our case, using the function `x()`
Read http://solidity.readthedocs.io/en/v0.4.24/contracts.html#getter-functions for automatic getter for public variable state.

Have to set the nonce (otherwise error)

```bash
parity tools hash <(echo -n "x()")
0c55699c4769c0b08a59db8a53119b799ea10ec55a4da92af76b0404363e0980
```

```bash
curl --data '{"method":"private_call","params":["latest",{"from":"0xe5a4b6f39b4c3e7203ca8caeecbad58d8f29b046","to":"0x4466399893d182dcbe43c2a5a79db93b52ed26ae","data":"0x0c55699c", "nonce":"0x18"}],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST localhost:8545
```

```json
{"jsonrpc":"2.0","result":"0x0000000000000000000000000000000000000000000000000000000000000001","id":1}
```

SetX(42):
```bash
parity tools hash <(echo -n "setX(bytes32)")
bc64b76d46f38211b46cd1a107337ffa1958d706b339b2a10aee4ea3d594b474
```

Compose
```bash
curl --data '{"method":"parity_composeTransaction","params":[{"from":"0xe5a4b6f39b4c3e7203ca8caeecbad58d8f29b046","to":"0x4466399893d182dcbe43c2a5a79db93b52ed26ae","data":"0xbc64b76d2a00000000000000000000000000000000000000000000000000000000000000"}],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST localhost:8545
```
Result:
```json
{
    "jsonrpc": "2.0",
    "result": {
        "condition": null,
        "data": "0xbc64b76d2a00000000000000000000000000000000000000000000000000000000000000",
        "from": "0xe5a4b6f39b4c3e7203ca8caeecbad58d8f29b046",
        "gas": "0xe57e0",
        "gasPrice": "0x0",
        "nonce": "0x18",
        "to": "0x4466399893d182dcbe43c2a5a79db93b52ed26ae",
        "value": "0x0"
    },
    "id": 1
}
```

Sign
```bash
curl --data '{"method":"personal_signTransaction","params":[{"condition":null,"data":"0xbc64b76d2a00000000000000000000000000000000000000000000000000000000000000","from":"0xe5a4b6f39b4c3e7203ca8caeecbad58d8f29b046","gas":"0xe57e0","gasPrice":"0x0","nonce":"0x18","to":"0x4466399893d182dcbe43c2a5a79db93b52ed26ae","value":"0x0"},"alicepwd"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST localhost:8545
```
```json
{
    "jsonrpc": "2.0",
    "result": {
        "raw": "0xf8841880830e57e0944466399893d182dcbe43c2a5a79db93b52ed26ae80a4bc64b76d2a0000000000000000000000000000000000000000000000000000000000000046a070587f5180901cca38c7b9d5b7e9d408ade342fa3f6cc16fc34c8e1df0bfe7eda040d8c3bbc695a4ab5c8fcbfd7f35b7b9e6352e5fb4a5113f63f1f67ab6d38b32",
        "tx": {
            "blockHash": null,
            "blockNumber": null,
            "chainId": "0x11",
            "condition": null,
            "creates": null,
            "from": "0xe5a4b6f39b4c3e7203ca8caeecbad58d8f29b046",
            "gas": "0xe57e0",
            "gasPrice": "0x0",
            "hash": "0x13724c166e0eba0e9a3adc32d8887494eba877bdaaca4ac8471014b87ac13e4f",
            "input": "0xbc64b76d2a00000000000000000000000000000000000000000000000000000000000000",
            "nonce": "0x18",
            "publicKey": "0xc20eea90083d48d53356423038056bc896b5f63ace0c4a8861deaffc5130da7747c7a2274165060f8f39c707e15dea47098a402e05c5e326b8d4f07d896edd86",
            "r": "0x70587f5180901cca38c7b9d5b7e9d408ade342fa3f6cc16fc34c8e1df0bfe7ed",
            "raw": "0xf8841880830e57e0944466399893d182dcbe43c2a5a79db93b52ed26ae80a4bc64b76d2a0000000000000000000000000000000000000000000000000000000000000046a070587f5180901cca38c7b9d5b7e9d408ade342fa3f6cc16fc34c8e1df0bfe7eda040d8c3bbc695a4ab5c8fcbfd7f35b7b9e6352e5fb4a5113f63f1f67ab6d38b32",
            "s": "0x40d8c3bbc695a4ab5c8fcbfd7f35b7b9e6352e5fb4a5113f63f1f67ab6d38b32",
            "standardV": "0x1",
            "to": "0x4466399893d182dcbe43c2a5a79db93b52ed26ae",
            "transactionIndex": null,
            "v": "0x46",
            "value": "0x0"
        }
    },
    "id": 1
}
```

Sent private

```bash
curl --data '{"method":"private_sendTransaction","params":["0xf8841880830e57e0944466399893d182dcbe43c2a5a79db93b52ed26ae80a4bc64b76d2a0000000000000000000000000000000000000000000000000000000000000046a070587f5180901cca38c7b9d5b7e9d408ade342fa3f6cc16fc34c8e1df0bfe7eda040d8c3bbc695a4ab5c8fcbfd7f35b7b9e6352e5fb4a5113f63f1f67ab6d38b32"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST localhost:8545 
```

```bash
{
    "jsonrpc": "2.0",
    "result": {
        "contractAddress": null,
        "status": 0,
        "transactionHash": "0x13724c166e0eba0e9a3adc32d8887494eba877bdaaca4ac8471014b87ac13e4f"
    },
    "id": 1
}
```

Call x() again to see
```bash
curl --data '{"method":"private_call","params":["latest",{"from":"0xe5a4b6f39b4c3e7203ca8caeecbad58d8f29b046","to":"0x4466399893d182dcbe43c2a5a79db93b52ed26ae","data":"0x0c55699c", "nonce":"0x19"}],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST localhost:8545
```
```json
{
    "jsonrpc": "2.0",
    "result": "0x2a00000000000000000000000000000000000000000000000000000000000000",
    "id": 1
}
```

|[ ← Part 3 - Private contract deployment ](Private-Transactions-Tutorial-3.md)|


