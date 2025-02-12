---
description: >-
  Example code for the eth_getTransactionByHash json-rpc method. Сomplete guide
  on how to use eth_getTransactionByHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionByHash - Fuse Network

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
curl --location --request POST 'https://fuse.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByHash",
"params": ["0x2a0a813a6e9be82009ff572656cf8bc04fad290405aa70fd18fd101b303911fd"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x21056b00f69c1d8f9e0caf94914efe3119cb1668163a9e432c01a9f900e37249",
        "blockNumber": "0xc63461",
        "chainId": "0x7a",
        "condition": null,
        "creates": null,
        "from": "0xee1f8da0139fba1ead54ea3e50033e075237aa6f",
        "gas": "0x164e6",
        "gasPrice": "0x3b9aca00",
        "hash": "0x2a0a813a6e9be82009ff572656cf8bc04fad290405aa70fd18fd101b303911fd",
        "input": "0x4e71d92d",
        "nonce": "0xe5",
        "publicKey": "0x857fa49917103d12432da447bf8d1ffd0b8e2372a565f0eab19bb308a3e18eaa2b969ef1f5886fe7268312828657bd63e4ef0d0a372a9a65e4d1efc94339868f",
        "r": "0x8b921315ebc8826da618758b52e1a93606e462621deb2496c5f835629dfaf8b1",
        "raw": "0xf86b81e5843b9aca00830164e694d7ac544f8a570c4d8764c3aabcf6870cbd960d0d80844e71d92d820118a08b921315ebc8826da618758b52e1a93606e462621deb2496c5f835629dfaf8b1a06ecb0113d67c3b0fff8775ffbb72ad93dbe2b5a8aebf5995307cb500aedb3c0d",
        "s": "0x6ecb0113d67c3b0fff8775ffbb72ad93dbe2b5a8aebf5995307cb500aedb3c0d",
        "standardV": "0x1",
        "to": "0xd7ac544f8a570c4d8764c3aabcf6870cbd960d0d",
        "transactionIndex": "0x0",
        "type": "0x0",
        "v": "0x118",
        "value": "0x0"
    }
}
```
