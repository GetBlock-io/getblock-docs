---
description: >-
  Example code for the settxfee  {disallowed} json-rpc method. Сomplete guide on
  how to use settxfee  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# settxfee {disallowed} - Bitcoin

#### Parameters

`amount` - numeric or string, required

The transaction fee in BTC/kvB

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "settxfee",
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
