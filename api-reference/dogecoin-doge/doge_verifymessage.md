---
description: >-
  Example code for the verifymessage json-rpc method. Сomplete guide on how to
  use verifymessage json-rpc in GetBlock.io Web3 documentation.
---

# verifymessage - Dogecoin

#### Parameters

`address` - string

Dogecoin address used to sign a message.

`signature` - string

The signature.

`message` - string

The message to sign.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "verifymessage",
"params": ["DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH", "mysignature", "mymessage"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": true
}
```
