---
description: >-
  Example code for the fundrawtransaction json-rpc method. Сomplete guide on how
  to use fundrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# fundrawtransaction - Zcash

#### Parameters

`hexstring` - string

The hex string of the raw transaction.

`includeWatching` - boolean

Optional, default=false

Also select inputs which are watch only.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "fundrawtransaction",
"params": [null, null],
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
