---
description: >-
  Example code for the signmessage json-rpc method. Сomplete guide on how to use
  signmessage json-rpc in GetBlock.io Web3 documentation.
---

# signmessage - Dogecoin

#### Parameters

`address` - string

Dogecoin address used to sign a message

`message` - string

The message to sign.

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "signmessage",
"params": ["DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH", "mymessage"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": null
}
```
