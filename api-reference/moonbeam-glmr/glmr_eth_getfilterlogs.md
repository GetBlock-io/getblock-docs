---
description: >-
  Example code for the eth_getFilterLogs json-rpc method. Сomplete guide on how
  to use eth_getFilterLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getFilterLogs - Moonbeam

#### Parameters

`QUANTITY` - string

The filter id.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getFilterLogs",
"params": ["0x16"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32603,
        "message": "Filter id 22 does not exist."
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
