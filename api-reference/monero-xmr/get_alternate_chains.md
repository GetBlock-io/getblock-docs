---
description: >-
  Example code for the get_alternate_chains json-rpc method. Сomplete guide on
  how to use get_alternate_chains json-rpc in GetBlock.io Web3 documentation.
---

# get\_alternate\_chains - Monero

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://xmr.getblock.io/mainnet/json_rpc' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "get_alternate_chains",
"params": {},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "chains": [
            {
                "block_hash": "f0a32bde81067dea27d53e20a5772c38a76b8eaf5b5bb417f43b234362e98bf9",
                "block_hashes": [
                    "f0a32bde81067dea27d53e20a5772c38a76b8eaf5b5bb417f43b234362e98bf9"
                ],
                "difficulty": 107546413585775815,
                "difficulty_top64": 0,
                "height": 2366090,
                "length": 1,
                "main_chain_parent_block": "26a1ff791429ca5f0e840669307acb3c74eb243cb7a460dc93c5b1f30233adac",
                "wide_difficulty": "0x17e14e4b2a5bcc7"
            }
        ],
        "status": "OK",
        "untrusted": false
    }
}
```
