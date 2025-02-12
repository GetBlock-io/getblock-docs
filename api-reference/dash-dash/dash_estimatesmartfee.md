---
description: >-
  Example code for the estimatesmartfee json-rpc method. Сomplete guide on how
  to use estimatesmartfee json-rpc in GetBlock.io Web3 documentation.
---

# estimatesmartfee - Dash

#### Parameters

`conf_target` - number (int)

Confirmation target in blocks (1 - 1008).

`estimate_mode` - string

Optional, default=CONSERVATIVE

The fee estimate mode. Whether to return a more conservative estimate which also satisfies a longer history.

A conservative estimate potentially returns a higher feerate and is more likely to be sufficient for the desired target, but is not as responsive to short term drops in the prevailing fee market.

Must be one of: - UNSET (defaults to CONSERVATIVE) - ECONOMICAL - CONSERVATIVE

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "estimatesmartfee",
"params": [100, "CONSERVATIVE"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "blocks": 100,
        "feerate": 1e-05
    }
}
```
