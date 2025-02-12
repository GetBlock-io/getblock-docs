---
description: >-
  Example code for the getblockheader json-rpc method. Сomplete guide on how to
  use getblockheader json-rpc in GetBlock.io Web3 documentation.
---

# getblockheader - Dash

#### Parameters

`header hash` - string (hex)

The hash of the block header to get, encoded as hex in RPC byte order.

`format` - boolean

Optional.

Set to false to get the block header in serialized block format; set to true (the default) to get the decoded block header as a JSON object.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockheader",
"params": ["00000000000000093711aeacfe1a827cb43c6d626230cdd2e41ad6f43c1e79d3", true],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bits": "192ab4b3",
        "chainlock": true,
        "chainwork": "000000000000000000000000000000000000000000006150154939c35ffff276",
        "confirmations": 570,
        "difficulty": 100569386.2568106,
        "hash": "00000000000000093711aeacfe1a827cb43c6d626230cdd2e41ad6f43c1e79d3",
        "height": 1535345,
        "mediantime": 1631177973,
        "merkleroot": "f10d0de8fa4dc0f60cc6028a7f9caaff7927d9fe00114d8211e1527e557fdced",
        "nTx": 8,
        "nextblockhash": "0000000000000003233fbb738a1bf5e2876face7b801b020d276b9c6c3967c67",
        "nonce": 1119488789,
        "previousblockhash": "000000000000002c086772e3101b2e598d244e20b73211d1dd86c59584dc4407",
        "time": 1631178288,
        "version": 536870912,
        "versionHex": "20000000"
    }
}
```
