---
description: >-
  Example code for the state_getStorageHash  {disallowed} json-rpc method.
  Сomplete guide on how to use state_getStorageHash  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# state\_getStorageHash {disallowed} - Polkadot

#### Parameters

`key` - StorageKey

storage key

`at` - BlockHash

block hash

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "state_getStorageHash",
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
