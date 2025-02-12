---
description: >-
  Example code for the eth_getLogs json-rpc method. Сomplete guide on how to use
  eth_getLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getLogs - Gnosis

#### Parameters

`Object` - hex string

Filter options object.

#### Request

```java
curl --location --request POST 'https://gno.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getLogs",
"params": [{"fromBlock": "0x1B13C84", "toBlock": "0x1B13C84", "address": "0xe9e7cea3dedca5984780bafc599bd69add087d56", "topics": []}],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32001,
        "message": "28392580 could not be found"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
