---
description: >-
  Example code for the eth_submitWork json-rpc method. Сomplete guide on how to
  use eth_submitWork json-rpc in GetBlock.io Web3 documentation.
---

# eth\_submitWork - Ethereum Classic

#### Parameters

`DATA, 8 Bytes` - string

Retrieved nonce.

`DATA, 32 Bytes` - string

Hash of the block header (PoW-hash).

`DATA, 32 Bytes` - string

Mix digest.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_submitWork",
"params": [1, 8234104122482341265491137074636836252947884782870784360943022469005013929455, "0xD1GE5700000000000000000000000000D1GE5700000000000000000000000000"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": true
}
```
