---
description: >-
  Example code for the /account/lock  {disallowed} rest method. Сomplete guide
  on how to use /account/lock  {disallowed} rest in GetBlock.io Web3
  documentation.
---

# /account/lock {disallowed} - NEM

#### Parameters

`privateKey` -

A PrivateKey JSON object. A private key is a key to an account. Anyone having the private key to an account can initiate any account related action. Therefore a private key must be kept secret at all costs.

#### Request

```java
curl --location --request POST 'https://xem.getblock.io/account/lock'
 --header 'x-api-key: YOUR-API-KEY'
 --header 'Content-Type: application/json'
 --data-raw '{'privateKey':{'value':'68e4f79f886927de698df4f857de2aada41ccca6617e56bb0d61623b35b08cc0'}}'
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
