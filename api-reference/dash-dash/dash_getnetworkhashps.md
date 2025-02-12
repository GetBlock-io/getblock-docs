---
description: >-
  Example code for the getnetworkhashps json-rpc method. Сomplete guide on how
  to use getnetworkhashps json-rpc in GetBlock.io Web3 documentation.
---

# getnetworkhashps - Dash

#### Parameters

`blocks` - number (int)

Optional.

The number of blocks to average together for calculating the estimated hashes per second.

Default is 120. Use -1 to average all blocks produced since the last difficulty change.

`height` - number (int)

Optional.

The height of the last block to use for calculating the average. Defaults to -1 for the highest-height block on the local best block chain.

If the specified height is higher than the highest block on the local best block chain, it will be interpreted the same as -1

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getnetworkhashps",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": 2972518180727094
}
```
