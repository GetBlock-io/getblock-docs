---
description: >-
  Example code for the eth_getTransactionByHash json-rpc method. Сomplete guide
  on how to use eth_getTransactionByHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionByHash - Rootstock

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getTransactionByHash",
"params": ["0x9683445fc38c4c33a15a7bea1d6b35bbb90874b97136563e8faf036f884f77e0"],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xf2a2b854721d4372474fc76cc13445a73369c0c334f4935c88bde3c310f28c9a",
        "blockNumber": "0x3833b4",
        "from": "0x90070628f8244fb2ba509c0184ba68d89bf0adad",
        "gas": "0x30d40",
        "gasPrice": "0x44fb308",
        "hash": "0x9683445fc38c4c33a15a7bea1d6b35bbb90874b97136563e8faf036f884f77e0",
        "input": "0x095ea7b30000000000000000000000005a0d867e0d70fcc6ade25c3f1b89d618b5b4eaa7ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "nonce": "0xd",
        "r": "0x23597cfa09c50c07fb1a80f109b75bebb30134017801ec28adbddbd34b7e136c",
        "s": "0x3569307e82e5712be4e4cba5a7f496bd97fb87a3eee4d4993aebbd8d8b67f012",
        "to": "0xe700691da7b9851f2f35f8b8182c69c53ccad9db",
        "transactionIndex": "0x0",
        "type": "0x0",
        "v": "0x5f",
        "value": "0x0"
    }
}
```
