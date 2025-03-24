---
description: >-
  Example code for the getmempoolentry json-rpc method. Сomplete guide on how to
  use getmempoolentry json-rpc in GetBlock.io Web3 documentation.
---

# getmempoolentry - DigiByte

#### Parameters

`txid` - string, required

The transaction id (musFot be in mempool)

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getmempoolentry",
"params": ["b63e5ee86d5407718083f2642bd6ca8d6425de315e3cbbc7e478c99f9b295056"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "fees": {
            "base": 0.001,
            "modified": 0.001,
            "ancestor": 0.001,
            "descendant": 0.001
        },
        "vsize": 225,
        "weight": 900,
        "fee": 0.001,
        "modifiedfee": 0.001,
        "time": 1617958131,
        "height": 678425,
        "descendantcount": 1,
        "descendantsize": 225,
        "descendantfees": 100000,
        "ancestorcount": 1,
        "ancestorsize": 225,
        "ancestorfees": 100000,
        "wtxid": "d56bc87e605bef45e1e63f43ece2a08edc80c65c709c7be33df6fa3c7f34688b",
        "depends": [],
        "spentby": [],
        "bip125-replaceable": false,
        "unbroadcast": false
    }
}
```
