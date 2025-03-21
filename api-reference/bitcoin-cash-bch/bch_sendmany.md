---
description: >-
  Example code for the sendmany  {disallowed} json-rpc method. Сomplete guide on
  how to use sendmany  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# sendmany {disallowed} - Bitcoin Cash

#### Parameters

`dummy` - string, required

Must be set to “” for backwards compatibility.

`amounts` - json object, required

The addresses and amounts

`minconf` - numeric, optional

Ignored dummy value

`comment` - string, optional

A comment

`subtractfeefrom` - json array, optional

The fee will be equally deducted from the amount of each selected address. Those recipients will receive less bitcoins than you enter in their corresponding amount field. If no addresses are specified here, the sender pays the fee.

`replaceable` - boolean, optional, default=wallet default

Allow this transaction to be replaced by a transaction with higher fees via BIP 125

`conf_target` - numeric, optional, default=wallet -txconfirmtarget

Confirmation target in blocks

`estimate_mode` - string, optional, default=unset

The fee estimate mode, must be one of (case insensitive) “unset” “economical” “conservative”

`fee_rate` - numeric or string, optional, default=not set, fall back to wallet fee estimation

Specify a fee rate in sat/vB.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "sendmany",
"params": [null, null, null, null, null, null, null, null, null],
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
