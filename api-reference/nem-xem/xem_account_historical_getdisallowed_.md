---
description: >-
  Example code for the /account/historical/get  {disallowed} rest method.
  Сomplete guide on how to use /account/historical/get  {disallowed} rest in
  GetBlock.io Web3 documentation.
---

# /account/historical/get {disallowed} - NEM

#### Parameters

`address` -

The address of the account.

`startHeight` -

The block height from which on the data should be supplied.

`endHeight` -

The block height up to which the data should be supplied. The end height must be greater than or equal to the start height.

`increment` -

The value by which the height is incremented between each data point. The value must be greater than 0. NIS can supply up to 1000 data points with one request. Requesting more than 1000 data points results in an error.

#### Request

```java
curl --location --request GET 'https://xem.getblock.io/account/historical/get?address=NCXIQA4FF5JB6AMQ53NQ3ZMRD3X3PJEWDJJJIGHT&startHeight=3943565&endHeight=3943570&increment=1'
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
