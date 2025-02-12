---
description: >-
  Example code for the sync_state_genSyncSpec  {disallowed} json-rpc method.
  Сomplete guide on how to use sync_state_genSyncSpec  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# sync\_state\_genSyncSpec {disallowed} - Kusama

#### Parameters

`raw` - bool

None

#### Request

```java
curl --location --request POST 'https://ksm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "sync_state_genSyncSpec",
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
