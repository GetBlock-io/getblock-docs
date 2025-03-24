---
description: >-
  Example code for the createrawtransaction json-rpc method. Сomplete guide on
  how to use createrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# createrawtransaction - Dash

#### Parameters

`inputs` - array

An array of objects, each one to be used as an input to the transaction

`Outputs` - array

A JSON array with outputs as key-value pairs.

`Locktime` - numeric (int)

Optional.

Added in Bitcoin Core 0.12.0

Indicates the earliest time a transaction can be added to the block chain.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "createrawtransaction",
"params": [[{"txid": "061ec99eb641ffdeaa05a1a724a255103bebc445b15c6c8c028b19c08608496b", "vout": 1}], [{"ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv": 800}, {"yY6AmGopsZS31wy1JLHR9P6AC6owFaXwuh": 74.99}], 0],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "02000000016b490886c0198b028c6c5cb145c4eb3b1055a224a7a105aadeff41b69ec91e060100000000ffffffff0200205fa0120000001976a914485485425fa99504ec1638ac4213f3cfc9f32ef388acc0a8f9be010000001976a914811eacc14db8ebb5b64486dc43400c0226b428a488ac00000000"
}
```
