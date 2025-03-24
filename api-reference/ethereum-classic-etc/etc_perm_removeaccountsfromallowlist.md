---
description: >-
  Example code for the perm_removeAccountsFromAllowlist  {disallowed} json-rpc
  method. Сomplete guide on how to use perm_removeAccountsFromAllowlist 
  {disallowed} json-rpc in GetBlock.io Web3 documentat
---

# perm\_removeAccountsFromAllowlist {disallowed} - Ethereum Classic

#### Parameters

`list of strings` - None

List of account addresses.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "perm_removeAccountsFromAllowlist",
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
