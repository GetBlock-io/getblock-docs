---
description: >-
  Example code for the getmempoolentry json-rpc method. Сomplete guide on how to
  use getmempoolentry json-rpc in GetBlock.io Web3 documentation.
---

# getmempoolentry - Dash

#### Parameters

`TXID` - string (hex)

The TXID of a transaction in the memory pool, encoded as hex in RPC byte order.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getmempoolentry",
"params": ["dc63e7f6929658feade06fec1eeaf43b"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "fees": {
            "base": 3.74e-06,
            "modified": 3.74e-06,
            "ancestor": 3.74e-06,
            "descendant": 6e-06
        },
        "size": 373,
        "fee": 3.74e-06,
        "modifiedfee": 3.74e-06,
        "time": 1610552403,
        "height": 425538,
        "descendantcount": 2,
        "descendantsize": 599,
        "descendantfees": 600,
        "ancestorcount": 1,
        "ancestorsize": 373,
        "ancestorfees": 374,
        "depends": [],
        "spentby": [
            "dc63e7f6929658feade06fec1eeaf43b3160095d66a9b59f57e77e56c20241fc"
        ],
        "instantlock": true
    }
}
```
