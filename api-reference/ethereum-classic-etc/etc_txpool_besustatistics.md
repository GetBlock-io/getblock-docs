---
description: >-
  Example code for the txpool_besuStatistics json-rpc method. Сomplete guide on
  how to use txpool_besuStatistics json-rpc in GetBlock.io Web3 documentation.
---

# txpool\_besuStatistics - Ethereum Classic

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "txpool_besuStatistics",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "maxSize": 4096,
        "localCount": 1,
        "remoteCount": 0
    }
}
```
