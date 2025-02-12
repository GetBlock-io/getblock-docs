---
description: >-
  Example code for the signrawtransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use signrawtransaction  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# signrawtransaction {disallowed} - Dash

#### Parameters

`Transaction` - string (hex)

The transaction to sign as a serialized transaction.

`Dependencies` - array

Optional.

The previous outputs being spent by this transaction.

{ "txid":"id" (string) - The TXID of the transaction the output appeared in. The TXID must be encoded in hex in RPC byte order. "vout":n (numeric) - The index number of the output (vout) as it appeared in its transaction, with the first output being 0. "scriptPubKey":"key" (string) - The output's pubkey script encoded as hex. "redeemScript":"script" (string, optional) - If the pubkey script was a script hash, this must be the corresponding redeem script. "amount":n (numeric) - The amount of Dash spent. }

`Private Keys` - array

Optional.

An array holding private keys. If any keys are provided, only they will be used to sign the transaction (even if the wallet has other matching keys). If this array is empty or not used, and wallet support is enabled, keys from the wallet will be used.

`SigHash` - string

Optional.

The type of signature hash to use for all of the signatures performed.

You must use separate calls to the signrawtransaction RPC if you want to use different signature hash types for different signatures. The allowed values are: ALL, NONE, SINGLE

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "signrawtransaction",
"params": ["03000500010000000000000000000000000000000000000000000000000000000000000000ffffffff2703716d170423ce39610800004440830900000fe4b883e5bda9e7a59ee4bb99e9b1bc04f09f909f40440fa802203d5807000000001976a9147c086eada12bdb10a265c16c08a7ae87366bd48188aca03c9f08000000001976a91406c7111117f7b797528485b64772d3ffcff919ec88ac209af41f460200716d1700efc371b5251f5bae393e5962fe092f8b2003732a56eda3e1a2babe8413d17ce7ce2396a41c1f833c0cd00a0d8e900dfc4962805706e70a35074dcd30fafbd4c6", null, null, "ALL"],
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
