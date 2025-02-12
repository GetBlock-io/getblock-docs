---
description: >-
  Example code for the /v1/accounts/{account_hash}/modules json-rpc method.
  Сomplete guide on how to use /v1/accounts/{account_hash}/modules json-rpc in
  GetBlock.io Web3 documentation.
---

# /v1/accounts/{account\_hash}/modules - Aptos

#### Parameters

`limit` -

Maximum number of resources to retrieve. Gets default page size if not provided.

`ledger_version` -

Ledger version. Defaults to latest if not provided.

`start` -

Optional cursor specifying pagination start. You can call this endpoint once without this parameter, and then use the cursor returned in the X-Aptos-Cursor header in the response.

#### Request

```java
curl --location --request GET 'https://apt.getblock.io/v1/accounts/0xc20ea5a196c81d8d7aff814aa37f8a5823acffbc4193efd3b2aafc9ef2803255/modules?limit=10' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
```

#### Response

```java
[]
```
