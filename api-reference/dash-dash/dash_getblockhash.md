---
description: >-
  Example code for the getblockhash json-rpc method. Сomplete guide on how to
  use getblockhash json-rpc in GetBlock.io Web3 documentation.
---

# getblockhash - Dash

#### Parameters

`block height` - number (int)

The height of the block whose header hash should be returned. The height of the hardcoded genesis block is 0.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockhash",
"params": [1535345],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "00000000000000093711aeacfe1a827cb43c6d626230cdd2e41ad6f43c1e79d3"
}
```
