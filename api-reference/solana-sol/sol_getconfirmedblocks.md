---
description: >-
  Example code for the getConfirmedBlocks  {disallowed} json-rpc method.
  Сomplete guide on how to use getConfirmedBlocks  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# getConfirmedBlocks {disallowed} - Solana

#### Parameters

`start_slot` - u64

start\_slot, as u64 integer

`end_slot` - u64

Optional

end\_slot, as u64 integer

`commitment` - string

Optional.

Commitment; "processed" is not supported. If parameter not provided, the default is "finalized".

#### Request

```java
curl --location --request POST 'https://sol.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getConfirmedBlocks",
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
