---
description: >-
  Example code for the getblocksubsidy json-rpc method. Сomplete guide on how to
  use getblocksubsidy json-rpc in GetBlock.io Web3 documentation.
---

# getblocksubsidy - Zcash

#### Parameters

`height` - numeric

Optional.

The block height. If not provided, defaults to the current height of the chain.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblocksubsidy",
"params": [null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": {
        "fundingstreams": [
            {
                "recipient": "Electric Coin Company",
                "specification": "https://zips.z.cash/zip-0214",
                "value": 0.21875,
                "valueZat": 21875000,
                "address": "t3cEUQFG3KYnFG6qYhPxSNgGi3HDjUPwC3J"
            },
            {
                "recipient": "Zcash Foundation",
                "specification": "https://zips.z.cash/zip-0214",
                "value": 0.15625,
                "valueZat": 15625000,
                "address": "t3dvVE3SQEi7kqNzwrfNePxZ1d4hUyztBA1"
            },
            {
                "recipient": "Major Grants",
                "specification": "https://zips.z.cash/zip-0214",
                "value": 0.25,
                "valueZat": 25000000,
                "address": "t3XyYW8yBFRuMnfvm5KLGFbEVz25kckZXym"
            }
        ],
        "miner": 2.5,
        "founders": 0
    },
    "error": null,
    "id": "getblock.io"
}
```
