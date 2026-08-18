---
description: >-
  Example code for the listtransactions  {disallowed} json-rpc method. Сomplete
  guide on how to use listtransactions  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# listtransactions {disallowed} - Dogecoin

#### Parameters

`account` - string

Optional.

Account to list transactions from. Return transactions from all accounts if None.

`count` - integer

Number of transactions to return.

`from_` - integer

Skip the first \[from\_] transactions.

`address` - string

Receive address to consider

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "listtransactions",
"params": [null, 2, 0, null],
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
