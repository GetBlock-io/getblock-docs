---
description: >-
  Example code for the eth_uninstallFilter json-rpc method. Сomplete guide on
  how to use eth_uninstallFilter json-rpc in GetBlock.io Web3 documentation.
---

# eth\_uninstallFilter - Polygon

#### Parameters

`QUANTITY` - string

The filter id.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_uninstallFilter",
"params": ["0xb"],
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
