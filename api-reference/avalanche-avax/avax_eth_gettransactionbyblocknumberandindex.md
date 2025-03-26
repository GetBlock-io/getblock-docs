---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex json-rpc
  in GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Avalanche

#### Parameters

`QUANTITY|TAG` - string

block number or "latest", "earliest" or "pending"

`QUANTITY` - string

Transaction index position.

#### Request

```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getTransactionByBlockNumberAndIndex",
  "params": ["latest", "0x0"],
  "id": "getblock.io"
}'
```

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x746540c469f195d7e44d124abcf406b7bd1ea8863eba18783dcd54791862e506",
        "blockNumber": "0x1d612e5",
        "chainId": "0xa86a",
        "from": "0x45b4a3c4fef734724a1ccebf64d76218ec7d46eb",
        "gas": "0x186a0",
        "gasPrice": "0x174876e800",
        "hash": "0x98631d12bc5900df3e230d5fb7ef3bdfb3a322e160b4d870cf8aa89e0a45c36f",
        "input": "0xa9059cbb0000000000000000000000007e4aa755550152a522d9578621ea22edab20430800000000000000000000000000000000000000000000000000000000080befc0",
        "nonce": "0x0",
        "r": "0x450f1255d6345de794a38e885cdf9eb00b3c18d554dd8c8a0486ea7dd2cfcb9d",
        "s": "0x329ec0e4bf92afd279edd3c9715e5c0edc133c3f5de0ecea4b3f942183b14097",
        "to": "0x9702230a8ea53601f5cd2dc00fdbc13d4df4a8c7",
        "transactionIndex": "0x0",
        "type": "0x0",
        "v": "0x150f7",
        "value": "0x0"
    }
}
```
