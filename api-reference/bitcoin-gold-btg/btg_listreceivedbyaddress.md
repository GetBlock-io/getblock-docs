---
description: >-
  Example code for the listreceivedbyaddress  {disallowed} json-rpc method.
  Сomplete guide on how to use listreceivedbyaddress  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# listreceivedbyaddress {disallowed} - Bitcoin Gold

#### Parameters

`minconf` - numeric, optional, default=1

The minimum number of confirmations before payments are included.

`include_empty` - boolean, optional, default=false

Whether to include addresses that haven’t received any payments.

`include_watchonly` - boolean, optional, default=true for watch-only wallets, otherwise false

Whether to include watch-only addresses (see ‘importaddress’)

`address_filter` - string, optional

If present, only return information on this address.

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "listreceivedbyaddress",
"params": [null, null, null, null],
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
