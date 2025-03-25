---
description: >-
  Example code for the /account/status rest method. Сomplete guide on how to use
  /account/status rest in GetBlock.io Web3 documentation.
---

# /account/status - NEM

#### Parameters

`address` -

The address of the account.

#### Request

```java
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/account/status?address=NCXIQA4FF5JB6AMQ53NQ3ZMRD3X3PJEWDJJJIGHT' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "cosignatories": [],
    "cosignatoryOf": [],
    "remoteStatus": "ACTIVE",
    "status": "LOCKED"
}
```
