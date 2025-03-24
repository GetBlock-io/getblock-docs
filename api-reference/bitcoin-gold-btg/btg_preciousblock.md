---
description: >-
  Example code for the preciousblock json-rpc method. Сomplete guide on how to
  use preciousblock json-rpc in GetBlock.io Web3 documentation.
---

# preciousblock - Bitcoin Gold

#### Parameters

`blockhash` - string, required

the hash of the block to mark as precious

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "preciousblock",
"params": ["000000009ec5dd6b53858593718cacdaaec989aaf9028c68013947224712682e"],
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
