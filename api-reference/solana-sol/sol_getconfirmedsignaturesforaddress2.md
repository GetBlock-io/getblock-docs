---
description: >-
  Example code for the getConfirmedSignaturesForAddress2  {disallowed} json-rpc
  method. Сomplete guide on how to use getConfirmedSignaturesForAddress2 
  {disallowed} json-rpc in GetBlock.io Web3 document
---

# getConfirmedSignaturesForAddress2 {disallowed} - Solana

#### Parameters

`address` - string

account address as base-58 encoded string

`config` - object

Optional

Configuration object containing the following fields: - limit: number (optional) - maximum transaction signatures to return (between 1 and 1,000, default: 1,000). - before: string (optional) - start searching backwards from this transaction signature. If not provided the search starts from the top of the highest max confirmed block. - until: string (optional) - search until this transaction signature, if found before limit reached. - commitment (optional) - "processed" is not supported. If parameter not provided, the default is "finalized".

#### Request

```java
curl --location --request POST 'https://sol.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getConfirmedSignaturesForAddress2",
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
