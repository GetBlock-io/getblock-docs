---
description: >-
  Example code for the estimatesmartfee json-rpc method. Сomplete guide on how
  to use estimatesmartfee json-rpc in GetBlock.io Web3 documentation.
---

# estimatesmartfee - Bitcoin SV

#### Parameters

`conf_target` - numeric, required

Confirmation target in blocks (1 - 1008)

`estimate_mode` - string, optional, default=CONSERVATIVE

The fee estimate mode.

Whether to return a more conservative estimate which also satisfies a longer history.

A conservative estimate potentially returns a higher feerate and is more likely to be sufficient for the desired target, but is not as responsive to short term drops in the prevailing fee market.

Must be one of:

UNSET

ECONOMICAL

CONSERVATIVE

#### Request

```java
curl --location --request POST 'https://bsv.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "estimatesmartfee",
"params": [1, "default"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": {
        "feerate": 0.00243871,
        "blocks": 2
    },
    "error": null,
    "id": "getblock.io"
}
```
