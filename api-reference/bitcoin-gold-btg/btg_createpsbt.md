---
description: >-
  Example code for the createpsbt json-rpc method. Сomplete guide on how to use
  createpsbt json-rpc in GetBlock.io Web3 documentation.
---

# createpsbt - Bitcoin Gold

#### Parameters

`inputs` - json array, required

A json array of json objects

`outputs` - json array, required

a json array with outputs (key-value pairs), where none of the keys are duplicated.

`locktime` - numeric, optional, default=0

Raw locktime. Non-0 value also locktime-activates inputs

`replaceable` - boolean, optional, default=false

Marks this transaction as BIP125 replaceable.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "createpsbt",
"params": [[{"txid": "myid", "vout": 0}], [{"data": "00010203"}], null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": null
}
```
