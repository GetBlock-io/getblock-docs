---
description: >-
  Example code for the createrawtransaction json-rpc method. Сomplete guide on
  how to use createrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# createrawtransaction - Zcash

#### Parameters

`transactions` - string

A json array of json objects:

\[ { "txid":"id", (string, required) The transaction id "vout":n (numeric, required) The output number "sequence":n (numeric, optional) The sequence number } ,... ]

`addresses` - object

a json object with addresses as keys and amounts as values

`locktime` - numeric

Optional, default=0

Raw locktime. Non-0 value also locktime-activates inputs.

`expiryheight` - numeric

Optional

default=nextblockheight+20 for pre-Blossom.

default=nextblockheight+40 for post-Blossom.

Expiry height of transaction (if Overwinter is active).

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "createrawtransaction",
"params": [[{"txid": "5db76e43724d980e05b5b98c9a83ba2d7fa565b3aa22bc3c5728ea56d0ed7cee", "vout": 123, "secuence": 123456789}], [{"t1L2rjgGrvEqfrA5zqUca4GGxAeQg47CTpG": 1234}], null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "0400008085202f8901ee7cedd056ea28573cbc22aab365a57f2dba839a8cb9b5050e984d72436eb75d7b00000000ffffffff01009236bb1c0000001976a91417b04a8ede7164eccb961f46289305ec04014b6388ac00000000f21f15000000000000000000000000"
}
```
