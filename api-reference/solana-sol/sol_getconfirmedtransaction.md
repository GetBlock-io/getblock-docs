---
description: >-
  Example code for the getConfirmedTransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use getConfirmedTransaction  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# getConfirmedTransaction {disallowed} - Solana

#### Parameters

`signature` - string

transaction signature as base-58 encoded string

`config` - object

Optional.

Configuration object containing the following optional fields: - encoding: string (optional) - encoding for each returned Transaction, either "json", "jsonParsed", "base58" (slow), "base64". If parameter not provided, the default encoding is "json". "jsonParsed" encoding attempts to use program-specific instruction parsers to return more human-readable and explicit data in the transaction.message.instructions list. If "jsonParsed" is requested but a parser cannot be found, the instruction falls back to regular JSON encoding (accounts, data, and programIdIndex fields). - commitment (optional) - "processed" is not supported. If parameter not provided, the default is "finalized".

#### Request

```java
curl --location --request POST 'https://sol.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getConfirmedTransaction",
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
