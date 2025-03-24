---
description: >-
  Example code for the preciousblock json-rpc method. Сomplete guide on how to
  use preciousblock json-rpc in GetBlock.io Web3 documentation.
---

# preciousblock - Bitcoin SV

#### Parameters

`blockhash` - string, required

the hash of the block to mark as precious

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "preciousblock",
"params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428"],
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
