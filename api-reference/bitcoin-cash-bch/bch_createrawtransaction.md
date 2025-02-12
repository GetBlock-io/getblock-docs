---
description: >-
  Example code for the createrawtransaction json-rpc method. Сomplete guide on
  how to use createrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# createrawtransaction - Bitcoin Cash

#### Parameters

`inputs` - json array, required

A json array of json objects

`outputs` - json array, required

The outputs (key-value pairs), where none of the keys are duplicated.

`locktime` - numeric, optional, default=0

Raw locktime. Non-0 value also locktime-activates inputs

`replaceable` - boolean, optional, default=false

Marks this transaction as BIP125 replaceable.

#### Request

```java
curl --location --request POST 'https://bch.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "createrawtransaction",
"params": [[{"txid": "myid", "vout": 0}], [{"data": "00010203"}], null, null],
"id": "getblock.io"}'
```

#### Response

```java
null
```
