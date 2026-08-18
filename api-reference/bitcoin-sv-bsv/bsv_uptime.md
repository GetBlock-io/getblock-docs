---
description: >-
  Example code for the uptime json-rpc method. Сomplete guide on how to use
  uptime json-rpc in GetBlock.io Web3 documentation.
---

# uptime - Bitcoin SV

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "uptime",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": 52903
}
```
