---
description: >-
  Example code for the getblockheaders json-rpc method. Сomplete guide on how to
  use getblockheaders json-rpc in GetBlock.io Web3 documentation.
---

# getblockheaders - Dash

#### Parameters

`header hash` - string (hex)

The hash of the block header to get, encoded as hex in RPC byte order

`count` - number

Optional.

The number of block headers to get

`verbose` - boolean

Optional.

Set to false to get the block headers in serialized block format; set to true (the default) to get the decoded block headers as a JSON object

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockheaders",
"params": ["00000000000000093711aeacfe1a827cb43c6d626230cdd2e41ad6f43c1e79d3", 2, true],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        {
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
        },
        {
            "bits": "192b0d08",
            "chainlock": true,
            "chainwork": "0000000000000000000000000000000000000000000061501b3b843e4abfe01c",
            "confirmations": 569,
            "difficulty": 99763336.62605304,
            "hash": "0000000000000003233fbb738a1bf5e2876face7b801b020d276b9c6c3967c67",
            "height": 1535346,
            "mediantime": 1631178023,
            "merkleroot": "1040a7f9debde7aafb233eaf4af8d7358d4da5eb147d64ce1a146d293b945466",
            "nTx": 20,
            "nextblockhash": "0000000000000008dbf1daa69bafcc06d4886e0dcf4ab72cbb05574250ac4b9b",
            "nonce": 3915364214,
            "previousblockhash": "00000000000000093711aeacfe1a827cb43c6d626230cdd2e41ad6f43c1e79d3",
            "time": 1631178413,
            "version": 536870912,
            "versionHex": "20000000"
        }
    ]
}
```
