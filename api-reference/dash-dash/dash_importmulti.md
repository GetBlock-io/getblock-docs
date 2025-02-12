---
description: >-
  Example code for the importmulti  {disallowed} json-rpc method. Сomplete guide
  on how to use importmulti  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# importmulti {disallowed} - Dash

#### Parameters

`Imports` - array

An array of JSON objects, each one being an address or script to be imported.

`Option` - object

Optional.

JSON object with options regarding the import.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "importmulti",
"params": [[{"name": "Import", "type": "object", "description": ["A JSON object describing a particular import."], "value": [{"name": "scriptPubKey", "type": "string (hex)", "description": ["Optional.", "The script (string) to be imported. Must have either this field or address below."], "value": null}, {"name": "address", "type": "string (base58)", "description": ["Optional.", "The P2PKH or P2SH address to be imported. Must have either this field or scriptPubKey above."], "value": null}, {"name": "timestamp", "type": "number (int) / string", "description": ["The creation time of the key in Unix epoch time or the string \u201cnow\u201d to substitute the current synced block chain time. The timestamp of the oldest key will determine how far back block chain rescans need to begin.", "Specify now to bypass scanning for keys which are known to never have been used.", "Specify 0 to scan the entire block chain.", "Blocks up to 2 hours before the earliest key creation time will be scanned."], "value": null}, {"name": "redeemscript", "type": "string", "description": ["Optional.", "A redeem script. Only allowed if either the address field is a P2SH address or the scriptPubKey field is a P2SH scriptPubKey."], "value": null}, {"name": "pubkeys", "type": "array", "description": ["Optional.", "Array of strings giving pubkeys that must occur in the scriptPubKey or redeemscript."], "value": null}, {"name": "keys", "type": "array", "description": ["Optional.", "Array of strings giving private keys whose corresponding public keys must occur in the scriptPubKey or redeemscript."], "value": null}, {"name": "internal", "type": "bool", "description": ["Optional.", "Stating whether matching outputs should be treated as change rather than incoming payments. The default is false."], "value": null}, {"name": "watchonly", "type": "bool", "description": ["Optional.", "Stating whether matching outputs should be considered watched even when they're not spendable. This is only allowed if keys are empty. The default is false."], "value": null}, {"name": "label", "type": "string", "description": ["Optional.", "Label to assign to the address, only allowed with internal set to false. The default is an empty string (\u201c\u201d)."], "value": null}]}], [{"name": "rescan", "type": "bool", "description": ["Optional.", "Set to true (the default) to rescan the entire local block chain for transactions affecting any imported address or script. Set to false to not rescan after the import.", "Rescanning may take a considerable amount of time and may require re-downloading blocks if using block chain pruning."], "value": null}]],
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
