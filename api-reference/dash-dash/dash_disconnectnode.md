---
description: >-
  Example code for the disconnectnode  {disallowed} json-rpc method. Сomplete
  guide on how to use disconnectnode  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# disconnectnode {disallowed} - Dash

#### Parameters

`address` - string

The node you want to disconnect from as a string in the form of IP\_address:port.

Updated in Bitcoin Core 0.14.1

`nodeid` - number

Optional.

The node ID (see getpeerinfo for node IDs)

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
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
