---
description: >-
  Example code for the state_getChildStorage  {disallowed} json-rpc method.
  Сomplete guide on how to use state_getChildStorage  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# state\_getChildStorage {disallowed} - Polkadot

#### Parameters

`childStorageKey` - StorageKey

child storage key

`childDefinition` - StorageKey

child storage key definition

`childType` - u32

child type

`key` - StorageKey

storage key

`at` - BlockHash

block hash

#### Request

```java
curl --location --request POST 'https://dot.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "state_getChildStorage",
"params": [null, null, null, null, null],
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
