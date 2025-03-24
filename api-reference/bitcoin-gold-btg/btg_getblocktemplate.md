---
description: >-
  Example code for the getblocktemplate json-rpc method. Сomplete guide on how
  to use getblocktemplate json-rpc in GetBlock.io Web3 documentation.
---

# getblocktemplate - Bitcoin Gold

#### Parameters

`template_request` - json object, optional, default={}

Format of the template

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblocktemplate",
"params": [null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bits": "1d0287ff",
        "capabilities": [
            "proposal"
        ],
        "coinbaseaux": {
            "flags": ""
        },
        "coinbasevalue": 625000000,
        "curtime": 1631009575,
        "equihashk": 5,
        "equihashn": 144,
        "height": 702635,
        "longpollid": "000000007cc1fc836d0992202730ca71f77a8cf2a35974ca300dadbf8bc1e0902",
        "mintime": 1631008652,
        "mutable": [
            "time",
            "transactions",
            "prevblock"
        ],
        "noncerange": "00000000ffffffff",
        "previousblockhash": "000000007cc1fc836d0992202730ca71f77a8cf2a35974ca300dadbf8bc1e090",
        "rules": [
            "csv",
            "segwit"
        ],
        "sigoplimit": 80000,
        "sizelimit": 4000000,
        "target": "0000000287ff0000000000000000000000000000000000000000000000000000",
        "transactions": [],
        "vbavailable": {},
        "vbrequired": 0,
        "version": 536870912,
        "weightlimit": 4000000
    }
}
```
