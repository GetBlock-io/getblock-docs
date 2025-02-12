---
description: >-
  Example code for the /local/account/transfers/all  {disallowed} rest method.
  Сomplete guide on how to use /local/account/transfers/all  {disallowed} rest
  in GetBlock.io Web3 documentation.
---

# /local/account/transfers/all {disallowed} - NEM

#### Parameters

`page` -

The account private key transactions page contains data that NIS needs to retrieve a set of transactions from the database. The data includes the private key of the account for which transactions are retrieved. Use requests that use this structure only when NIS is running locally. The fields "hash" and "id" are optional.

example: { "value": "68e4f79f886927de698df4f857de2aada41ccca6617e56bb0d61623b35b08cc0", "hash": "44e4968e5aa35fe182d4def5958e23cf941c4bf809364afb4431ebbf6a18c039", "id": "12345" }

#### Request

```java
curl --location --request POST 'https://xem.getblock.io/local/account/transfers/all' 
 --header 'x-api-key: YOUR-API-KEY' 
 --header 'Content-Type: application/json'
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
