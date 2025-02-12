---
description: >-
  Example code for the getcoinjoininfo json-rpc method. Сomplete guide on how to
  use getcoinjoininfo json-rpc in GetBlock.io Web3 documentation.
---

# getcoinjoininfo - Dash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getcoinjoininfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "denoms_goal": 50,
        "denoms_hardcap": 300,
        "enabled": false,
        "max_amount": 1000,
        "max_rounds": 4,
        "max_sessions": 4,
        "multisession": false,
        "queue_size": 0
    }
}
```
