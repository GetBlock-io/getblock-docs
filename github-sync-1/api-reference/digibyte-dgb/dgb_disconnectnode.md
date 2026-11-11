---
description: >-
  Example code for the disconnectnode  {disallowed} json-rpc method. Сomplete
  guide on how to use disconnectnode  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# disconnectnode {disallowed} - DigiByte

#### Parameters

`address` - string, optional, default=fallback to nodeid

The IP address/port of the node

`nodeid` - numeric, optional, default=fallback to address

The node ID (see getpeerinfo for node IDs)

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "disconnectnode",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
