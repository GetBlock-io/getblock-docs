---
description: >-
  Example code for the post  {disallowed} json-rpc method. Сomplete guide on how
  to use post  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# post {disallowed} - Binance Smart Chain

#### Parameters

`from` - DATA

60 Bytes - (optional) The identity of the sender.

`to` - DATA

60 Bytes - (optional) The identity of the receiver. When present whisper will encrypt the message so that only the receiver can decrypt it.

`topics` - Array of DATA

Array of DATA topics, for the receiver to identify messages.

`payload` - DATA

The payload of the message.

`priority` - QUANTITY

The integer of the priority

`ttl` - QUANTITY

integer of the time to live in seconds.

#### Request

```java
curl --location --request POST 'https://bsc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "post",
"params": [null, null, null, null, null, null],
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
