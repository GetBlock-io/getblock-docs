---
description: >-
  Example code for the eth_uninstallFilter json-rpc method. Сomplete guide on
  how to use eth_uninstallFilter json-rpc in GetBlock.io Web3 documentation.
---

# eth\_uninstallFilter - Fantom

#### Parameters

`data` - hex string

Filter ID.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_uninstallFilter",
"params": ["0x6b08f09a8c82c83da98981500efd5540"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": false
}
```
