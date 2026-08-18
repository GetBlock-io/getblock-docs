---
description: >-
  Example code for the eth_getFilterLogs json-rpc method. Сomplete guide on how
  to use eth_getFilterLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getFilterLogs - Rootstock

#### Parameters

`QUANTITY` - string

The filter id.

#### Request

```java
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getFilterLogs",
"params": ["0x16"],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": null
}
```
