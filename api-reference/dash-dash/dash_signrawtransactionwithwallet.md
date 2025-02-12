---
description: >-
  Example code for the signrawtransactionwithwallet  {disallowed} json-rpc
  method. Сomplete guide on how to use signrawtransactionwithwallet 
  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# signrawtransactionwithwallet {disallowed} - Dash

#### Parameters

`Transaction` - string (hex)

The transaction to sign as a serialized transaction.

`Dependencies` - array

Optional.

The previous outputs being spent by this transaction.

`SigHash` - string

The type of signature hash to use for all of the signatures performed.

You must use separate calls to the signrawtransactionwithwallet RPC if you want to use different signature hash types for different signatures. The allowed values are: ALL, NONE, SINGLE.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "signrawtransactionwithwallet",
"params": [null, [{"name": "Output", "type": "object", "description": ["Optional.", "An output being spent."], "value": [{"name": "txid", "type": "string (hex)", "description": ["The TXID of the transaction the output appeared in. The TXID must be encoded in hex in RPC byte order."], "value": null}, {"name": "vout", "type": "number (int)", "description": ["The index number of the output (vout) as it appeared in its transaction, with the first output being 0."], "value": null}, {"name": "scriptPubKey", "type": "string (hex)", "description": ["The output's pubkey script encoded as hex."], "value": null}, {"name": "redeemScript", "type": "string (hex)", "description": ["Optional.", "If the pubkey script was a script hash, this must be the corresponding redeem script."], "value": null}, {"name": "amount", "type": "numeric", "description": ["The amount of Dash spent"], "value": null}]}], null],
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
