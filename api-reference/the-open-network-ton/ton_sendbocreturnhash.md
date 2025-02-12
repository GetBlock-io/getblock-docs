---
description: >-
  Example code for the /sendBocReturnHash json-rpc method. Сomplete guide on how
  to use /sendBocReturnHash json-rpc in GetBlock.io Web3 documentation.
---

# /sendBocReturnHash - The Open Network (TON)

#### Parameters

`boc` - body

string

serialized boc file, b64 encoded bag of cells

#### Request

```java
curl --location --request POST 'https://ton.getblock.io/mainnet/rest//sendBocReturnHash?' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{}'
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
