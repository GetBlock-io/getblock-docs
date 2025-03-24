---
description: >-
  Example code for the getmempoolancestors json-rpc method. Сomplete guide on
  how to use getmempoolancestors json-rpc in GetBlock.io Web3 documentation.
---

# getmempoolancestors - Dash

#### Parameters

`TXID` - string (hex)

The TXID of a transaction in the memory pool, encoded as hex in RPC byte order.

`Format` - boolean

Optional.

Set to true to get json objects describing each transaction in the memory pool; set to false (the default) to only get an array of TXIDs.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getmempoolancestors",
"params": ["dc63e7f6929658feade06fec1eeaf43b", false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "d64eb30e5435e7a4564df9d06525a8ab48858fdaf111661d1e7874a72cebc132"
    ]
}
```
