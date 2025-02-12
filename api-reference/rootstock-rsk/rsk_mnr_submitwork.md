---
description: >-
  Example code for the mnr_submitWork  {disallowed} json-rpc method. Сomplete
  guide on how to use mnr_submitWork  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# mnr\_submitWork {disallowed} - Rootstock

#### Parameters

`data` - string

The nonce found (64 bits)

`data` - string

The header’s pow-hash (256 bits)

`data` - string

The mix digest (256 bits)

#### Request

```java
wscat -c wss://rsk.getblock.io/YOUR-API-KEY/mainnet/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "mnr_submitWork",
"params": [null, null, null],
"id": "getblock.io"}
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
