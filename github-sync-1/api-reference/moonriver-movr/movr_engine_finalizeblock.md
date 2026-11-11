---
description: >-
  Example code for the engine_finalizeBlock  {disallowed} json-rpc method.
  Сomplete guide on how to use engine_finalizeBlock  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# engine\_finalizeBlock {disallowed} - Moonriver

#### Parameters

`hash` - BlockHash

None

`justification` - Justification

None

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "engine_finalizeBlock",
"params": [null, null],
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
