---
description: >-
  Example code for the sandbox_patch_state  {disallowed} json-rpc method.
  Сomplete guide on how to use sandbox_patch_state  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# sandbox\_patch\_state {disallowed} - NEAR Protocol

#### Parameters

`records` - array

an array of state records to patch. Every state record can be one of Account, AccessKey, Contract (for contract code), or Data (for contract state).

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "sandbox_patch_state",
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
