---
description: >-
  Example code for the getConfirmedBlock  {disallowed} json-rpc method. Сomplete
  guide on how to use getConfirmedBlock  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# getConfirmedBlock {disallowed} - Solana

#### Parameters

`slot` - u64

slot, as u64 integer

`Config` - object

Configuration object containing the following optional fields: - encoding: string (optional) - encoding for each returned Transaction, either "json", "jsonParsed", "base58" (slow), "base64". If parameter not provided, the default encoding is "json". "jsonParsed" encoding attempts to use program-specific instruction parsers to return more human-readable and explicit data in the transaction.message.instructions list. If "jsonParsed" is requested but a parser cannot be found, the instruction falls back to regular JSON encoding (accounts, data, and programIdIndex fields). - transactionDetails: string (optional) - level of transaction detail to return, either "full", "signatures", or "none". If parameter not provided, the default detail level is "full". - rewards: bool (optional) - whether to populate the rewards array. If parameter not provided, the default includes rewards. - commitment (optional) - "processed" is not supported. If parameter not provided, the default is "finalized".

#### Request

```java
curl --location --request POST 'https://sol.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getConfirmedBlock",
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
