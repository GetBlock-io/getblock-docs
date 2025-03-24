---
description: >-
  Example code for the getmempooldescendants json-rpc method. Сomplete guide on
  how to use getmempooldescendants json-rpc in GetBlock.io Web3 documentation.
---

# getmempooldescendants - Dash

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
"method": "getmempooldescendants",
"params": ["dc63e7f6929658feade06fec1eeaf43b", false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "dc63e7f6929658feade06fec1eeaf43b3160095d66a9b59f57e77e56c20241fc"
    ]
}
```
