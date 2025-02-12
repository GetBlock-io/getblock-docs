---
description: >-
  Example code for the lockunspent  {disallowed} json-rpc method. Сomplete guide
  on how to use lockunspent  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# lockunspent {disallowed} - Dash

#### Parameters

`Unlock` - bool

Set to false to lock the outputs specified in the following parameter.

Set to true to unlock the outputs specified.

If this is the only argument specified and it is set to true, all outputs will be unlocked; if it is the only argument and is set to false, there will be no change.

`Outputs` - array

Optional.

An array of outputs to lock or unlock.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "lockunspent",
"params": [null, [{"name": "Output", "type": "object", "description": ["An object describing a particular output."], "value": [{"name": "txid", "type": "string", "description": ["The TXID of the transaction containing the output to lock or unlock, encoded as hex in internal byte order."], "value": null}, {"name": "vout", "type": "number (int)", "description": ["The output index number (vout) of the output to lock or unlock. The first output in a transaction has an index of 0."], "value": null}]}]],
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
