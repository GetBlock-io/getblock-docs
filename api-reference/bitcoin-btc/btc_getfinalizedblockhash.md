---
description: >-
  Example code for the getfinalizedblockhash json-rpc method. Сomplete guide on
  how to use getfinalizedblockhash json-rpc in GetBlock.io Web3 documentation.
---

# getfinalizedblockhash - Bitcoin

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getfinalizedblockhash",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "000000000000000000073da808b05b4b94337d838b3e0a5b98f25b98363216c3"
}
```
