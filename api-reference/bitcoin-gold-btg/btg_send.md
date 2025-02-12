---
description: >-
  Example code for the send  {disallowed} json-rpc method. Сomplete guide on how
  to use send  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# send {disallowed} - Bitcoin Gold

#### Parameters

`outputs` - json array, required

The outputs (key-value pairs), where none of the keys are duplicated.

That is, each address can only appear once and there can only be one ‘data’ object. For convenience, a dictionary, which holds the key-value pairs directly, is also accepted.

`conf_target` - numeric, optional, default=wallet -txconfirmtarget

Confirmation target in blocks

`estimate_mode` - string, optional, default=unset

The fee estimate mode, must be one of (case insensitive) “unset” “economical” “conservative”

`fee_rate` - numeric or string, optional, default=not set, fall back to wallet fee estimation

Specify a fee rate in sat/vB.

`options` - json object, optional

“locktime” n, (numeric, optional, default=0) Raw locktime. Non-0 value also locktime-activates inputs

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "send",
"params": [null, null, null, null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
