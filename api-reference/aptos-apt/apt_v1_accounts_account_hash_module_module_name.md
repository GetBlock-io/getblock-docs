---
description: >-
  Example code for the /v1/accounts/{account_hash}/module/{module_name} json-rpc
  method. Сomplete guide on how to use
  /v1/accounts/{account_hash}/module/{module_name} json-rpc in GetBlock.io Web3
  docume
---

# /v1/accounts/{account\_hash}/module/{module\_name} - Aptos

#### Parameters

`module_name` -

A type of struct to retrieve.

`ledger_version` -

Ledger version. Defaults to latest if not provided.

#### Request

```java
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/v1/accounts/0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255/module/coin?module_name=coin' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "error_code": "module_not_found",
    "message": "Module not found by Address(0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255), Module name(coin) and Ledger version(152087584)",
    "vm_error_code": null
}
```
