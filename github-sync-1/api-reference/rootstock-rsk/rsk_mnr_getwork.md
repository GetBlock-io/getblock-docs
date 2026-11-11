---
description: >-
  Example code for the mnr_getWork json-rpc method. Сomplete guide on how to use
  mnr_getWork json-rpc in GetBlock.io Web3 documentation.
---

# mnr\_getWork - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "mnr_getWork",
"params": [],
"id": "getblock.io"}
```

#### Response

```java
{
    "error": {
        "code": -32601,
        "message": "the method mnr_getWork does not exist/is not available"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
