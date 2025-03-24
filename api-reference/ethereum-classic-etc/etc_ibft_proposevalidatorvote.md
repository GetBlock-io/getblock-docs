---
description: >-
  Example code for the ibft_proposeValidatorVote  {disallowed} json-rpc method.
  Сomplete guide on how to use ibft_proposeValidatorVote  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# ibft\_proposeValidatorVote {disallowed} - Ethereum Classic

#### Parameters

`data` - None

Account address

`boolean` - None

true to propose adding validator or false to propose removing validator.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "ibft_proposeValidatorVote",
"params": [null, null],
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
