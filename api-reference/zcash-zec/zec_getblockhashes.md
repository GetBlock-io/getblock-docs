---
description: >-
  Example code for the getblockhashes  {disallowed} json-rpc method. Сomplete
  guide on how to use getblockhashes  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getblockhashes {disallowed} - Zcash

#### Parameters

`high.` - numeric

The newer block timestamp.

`low` - numeric

The older block timestamp.

`options` - string

Optional.

A json object:

{

"noOrphans": true|false (boolean) will only include blocks on the main chain

"logicalTimes": true|false (boolean) will include logical timestamps with hashes

}

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockhashes",
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
