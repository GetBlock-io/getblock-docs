---
description: >-
  Example code for the getdifficulty json-rpc method. Сomplete guide on how to
  use getdifficulty json-rpc in GetBlock.io Web3 documentation.
---

# getdifficulty - Bitcoin Gold

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getdifficulty",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": 197682.9725910754
}
```
