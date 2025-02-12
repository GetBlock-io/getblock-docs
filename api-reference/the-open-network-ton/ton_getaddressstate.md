---
description: >-
  Example code for the /getAddressState json-rpc method. Сomplete guide on how
  to use /getAddressState json-rpc in GetBlock.io Web3 documentation.
---

# /getAddressState - The Open Network (TON)

#### Parameters

`address` - query

required, string

Identifier of target TON account in any form.

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet//getAddressState?address=EQDXZ2c5LnA12Eum-DlguTmfYkMOvNeFCh4rBD0tgmwjcFI-' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": "active"
}
```
