---
description: >-
  Example code for the getdifficulty json-rpc method. Сomplete guide on how to
  use getdifficulty json-rpc in GetBlock.io Web3 documentation.
---

# getdifficulty - Bitcoin

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
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
    "result": 303127737690.0432
}
```
