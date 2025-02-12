---
description: >-
  Example code for the /jsonRPC json-rpc method. Сomplete guide on how to use
  /jsonRPC json-rpc in GetBlock.io Web3 documentation.
---

# /jsonRPC - The Open Network (TON)

#### Parameters

`jsonprc` - string

jsonrpc version

`id` - string

request id

`method` - string

method name

`params` - json body

json body with method params

#### Request

```java
curl --location --request POST 'https://ton.getblock.io/mainnet/jsonRPC' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
--data-raw '{"jsonrpc": "2.0",
"method": "getConsensusBlock",
"id": "getblock.io"}'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "consensus_block": 30517987,
        "timestamp": 1687339459.355614
    },
    "jsonrpc": "2.0",
    "id": "getblock"
}
```
