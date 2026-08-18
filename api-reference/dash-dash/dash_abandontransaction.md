---
description: >-
  Example code for the abandontransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use abandontransaction  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# abandontransaction {disallowed} - Dash

#### Parameters

`TXID` - string (hex)

The TXID of the transaction that you want to abandon. The TXID must be encoded as hex in RPC byte order.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "abandontransaction",
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
