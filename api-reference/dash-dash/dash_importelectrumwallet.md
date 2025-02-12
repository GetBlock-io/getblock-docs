---
description: >-
  Example code for the importelectrumwallet  {disallowed} json-rpc method.
  Сomplete guide on how to use importelectrumwallet  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# importelectrumwallet {disallowed} - Dash

#### Parameters

`File name` - string

The Electrum wallet export file (should be in csv or json format).

`Index` - number (int)

Optional.

Rescan the wallet for transactions starting from this block index (default: 0)

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "importelectrumwallet",
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
