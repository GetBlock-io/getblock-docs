---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Rootstock

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
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
        "contractAddress": null,
        "cumulativeGasUsed": "0xb9b2",
        "from": "0x90070628f8244fb2ba509c0184ba68d89bf0adad",
        "gasUsed": "0xb9b2",
        "logs": [
            {
                "address": "0xe700691da7b9851f2f35f8b8182c69c53ccad9db",
                "blockHash": "0xf2a2b854721d4372474fc76cc13445a73369c0c334f4935c88bde3c310f28c9a",
                "blockNumber": "0x3833b4",
                "data": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
                "logIndex": "0x0",
                "topics": [
                    "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                    "0x00000000000000000000000090070628f8244fb2ba509c0184ba68d89bf0adad",
                    "0x0000000000000000000000005a0d867e0d70fcc6ade25c3f1b89d618b5b4eaa7"
                ],
                "transactionHash": "0x9683445fc38c4c33a15a7bea1d6b35bbb90874b97136563e8faf036f884f77e0",
                "transactionIndex": "0x0"
            }
        ],
        "logsBloom": "0x00000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400020000000000000000020000000000000000008000000000000000000000000000000000000000000000000000000000020000000002000000000000000008000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002000000012000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1",
        "to": "0xe700691da7b9851f2f35f8b8182c69c53ccad9db",
        "transactionHash": "0x9683445fc38c4c33a15a7bea1d6b35bbb90874b97136563e8faf036f884f77e0",
        "transactionIndex": "0x0",
        "type": "0x0"
    }
}
```
