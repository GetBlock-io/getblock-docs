---
description: >-
  Example code for the gettransaction  {disallowed} json-rpc method. Сomplete
  guide on how to use gettransaction  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# gettransaction {disallowed} - Dash

#### Parameters

`TXID` - string (hex)

The TXID of the transaction to get details about. The TXID must be encoded as hex in RPC byte order.

`Include Watch-Only` - bool

Optional.

If set to true, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.

If set to false (the default), treat watch-only addresses as if they didn't belong to this wallet.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "gettransaction",
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
