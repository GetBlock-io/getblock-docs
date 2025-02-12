---
description: >-
  Example code for the getspecialtxes json-rpc method. Сomplete guide on how to
  use getspecialtxes json-rpc in GetBlock.io Web3 documentation.
---

# getspecialtxes - Dash

#### Parameters

`blockhash` - string

The block hash.

`type` - int

Optional.

Filter special txes by type, -1 means all types (default: -1)

`count` - int

Optional.

The number of transactions to return (default: 10)

`skip` - int

Optional.

The number of transactions to skip (default: 0)

`verbosity` - int

Optional.

0 for hashes, 1 for hex-encoded data, and 2 for JSON object (default: 0)

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getspecialtxes",
"params": ["00000000000000093711aeacfe1a827cb43c6d626230cdd2e41ad6f43c1e79d3", -1, 10, 0, 0],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "83dc6c8e03026c0317885f62a7072dfde10014967f59477a0f7b5fc52f44a784"
    ]
}
```
