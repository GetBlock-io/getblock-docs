---
description: >-
  Example code for the getmempoolinfo json-rpc method. Сomplete guide on how to
  use getmempoolinfo json-rpc in GetBlock.io Web3 documentation.
---

# getmempoolinfo - Dash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getmempoolinfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bytes": 3353,
        "instantsendlocks": 21,
        "maxmempool": 300000000,
        "mempoolminfee": 1e-05,
        "minrelaytxfee": 1e-05,
        "size": 12,
        "usage": 14224
    }
}
```
