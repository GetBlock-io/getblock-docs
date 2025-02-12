---
description: >-
  Example code for the state_queryStorage  {disallowed} json-rpc method.
  Сomplete guide on how to use state_queryStorage  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# state\_queryStorage {disallowed} - Kusama

#### Parameters

`keys` - Vector of StorageKey

list of storage keys

`fromBlock` - Hash

start block hash

`toBlock` - BlockHash

end block hash

#### Request

```java
curl --location --request POST 'https://ksm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "state_queryStorage",
"params": [null, null, null],
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
