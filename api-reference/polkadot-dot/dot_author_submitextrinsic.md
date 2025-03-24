---
description: >-
  Example code for the author_submitExtrinsic  {disallowed} json-rpc method.
  Сomplete guide on how to use author_submitExtrinsic  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# author\_submitExtrinsic {disallowed} - Polkadot

#### Parameters

`extrinsic` - Extrinsic

None

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "author_submitExtrinsic",
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
