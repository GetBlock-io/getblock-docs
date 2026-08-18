---
description: >-
  Example code for the eth_newFilter json-rpc method. Сomplete guide on how to
  use eth_newFilter json-rpc in GetBlock.io Web3 documentation.
---

# eth\_newFilter - Rootstock

#### Parameters

`Object` - object

Filter options

#### Request

```java
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_newFilter",
"params": [[]],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x1abd0"
}
```
