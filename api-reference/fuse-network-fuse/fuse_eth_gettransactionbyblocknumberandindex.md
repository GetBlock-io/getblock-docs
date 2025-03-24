---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex json-rpc
  in GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Fuse Network

#### Parameters

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

`QUANTITY` - integer

Transaction index position.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockNumberAndIndex",
"params": ["latest", "0x0"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x89b1661295ad3d8c40c513b7aac32d36df415969e9e6f26ebc2756e6942980dd",
        "blockNumber": "0x167cdfd",
        "chainId": "0x7a",
        "condition": null,
        "creates": null,
        "from": "0xacadb09ac8cd006b9ed391d940f952659e3ce500",
        "gas": "0x1fe91",
        "gasPrice": "0x2540be400",
        "hash": "0x4a868765061a4254ea3f1c7b711f683cdd1ec2898fcf714583049a741a15c082",
        "input": "0x3771dcf8000000000000000000000000acadb09ac8cd006b9ed391d940f952659e3ce500",
        "nonce": "0x6f",
        "publicKey": "0xf5744fa71fd1e92cae7ae2c26e8cd4f84beb14d34d34ed608761ad024ecd4da812881846a78dcc0dbf79a3cb00f0a96f6a6dd1fb3d44ea43dc0d9996bdfaf0ff",
        "r": "0xbd659767185833010ae402c6a9be0a6123496e74bcf9be53066e20143e5862e8",
        "raw": "0xf88b6f8502540be4008301fe919401ab5966c1d742ae0cff7f14cc0f4d85156e83d980a43771dcf8000000000000000000000000acadb09ac8cd006b9ed391d940f952659e3ce500820118a0bd659767185833010ae402c6a9be0a6123496e74bcf9be53066e20143e5862e8a004f65f3b91aee017c88b48bfc31981755591cff9031ad416f8951e9c4e96739d",
        "s": "0x4f65f3b91aee017c88b48bfc31981755591cff9031ad416f8951e9c4e96739d",
        "standardV": "0x1",
        "to": "0x01ab5966c1d742ae0cff7f14cc0f4d85156e83d9",
        "transactionIndex": "0x0",
        "type": "0x0",
        "v": "0x118",
        "value": "0x0"
    }
}
```
