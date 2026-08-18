---
description: >-
  Example code for the setnetworkactive  {disallowed} json-rpc method. Сomplete
  guide on how to use setnetworkactive  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# setnetworkactive {disallowed} - Dash

#### Parameters

`Activate` - bool

Set to true to enable all P2P network activity.

Set to false to disable all P2P network activity

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "setnetworkactive",
"params": [null],
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
