---
description: >-
  Example code for the state_getReadProof  {disallowed} json-rpc method.
  Сomplete guide on how to use state_getReadProof  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# state\_getReadProof {disallowed} - Kusama

#### Parameters

`keys` - Vector of StorageKey

list of storage keys

`at` - BlockHash

block hash

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "state_getReadProof",
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
