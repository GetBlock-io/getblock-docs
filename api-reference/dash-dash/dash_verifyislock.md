---
description: >-
  Example code for the verifyislock  {disallowed} json-rpc method. Сomplete
  guide on how to use verifyislock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# verifyislock {disallowed} - Dash

#### Parameters

`id` - string (hex)

Signing request ID.

`txid` - string (hex)

The transaction id (TXID).

`signature` - string (hex)

The InstantSend Lock signature to verify.

`maxHeight` - number

Optional.

The maximum height to search quorums from.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "verifyislock",
"params": [null, null, null, null],
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
