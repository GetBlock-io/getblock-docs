---
description: >-
  Example code for the getblock json-rpc method. Сomplete guide on how to use
  getblock json-rpc in GetBlock.io Web3 documentation.
---

# getblock - Dash

#### Parameters

`block hash` - string (hex)

The hash of the header of the block to get, encoded as hex in RPC byte order.

`Verbosity` - number (int)

Optional.

Set to one of the following verbosity levels:

\- 0 - Get the block in serialized block format;

\- 1 - Get the decoded block as a JSON object (default)

\- 2 - Get the decoded block as a JSON object with transaction details

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblock",
"params": ["00000000000000093711aeacfe1a827cb43c6d626230cdd2e41ad6f43c1e79d3", 1],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bits": "192ab4b3",
        "cbTx": {
            "height": 1535345,
            "merkleRootMNList": "e77cd11384bebaa2e1a3ed562a7303208b2f09fe62593e39ae5b1f25b571c3ef",
            "merkleRootQuorums": "c6d4fbfa30cd4d07350ae70657806249fc0d908e0d0ad00c3c831f1ca49623ce",
            "version": 2
        },
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
        "size": 2692,
        "time": 1631178288,
        "tx": [
            "83dc6c8e03026c0317885f62a7072dfde10014967f59477a0f7b5fc52f44a784",
            "53fd7dcc171756ab56c1c9af76bdc398f6be69d97dcbe8696b53ff1b140c2249",
            "7d2d8aa7ee2e4c9d28bf390a4b3751fbd56b5bd3a9e3eb88cc767ce78dc8120b",
            "01108f50aa05893d5e4f5777a512fb0a87d784a3c7f3ce37f14caada5e347a4a",
            "8b31cd7e89a1246e2d2a6fbbc71cc0d5e4eeb40130f001607ce011eaa7081c25",
            "1cfe92a8aa091c7ab8a9b14239126fb12d28cfb02df5a941753d16cdebf6cc56",
            "c4c988dec1bfaaee340c3f176dce57cd40a074c3157142c471c1c33cb36d8d5f",
            "31eab81d2b5ffa6a5c3e4ded0414144057b82d04cef0731338fe96b4576de658"
        ],
        "version": 536870912,
        "versionHex": "20000000"
    }
}
```
