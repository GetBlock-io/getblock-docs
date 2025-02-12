---
description: >-
  Example code for the /v1/estimate_gas_price json-rpc method. Сomplete guide on
  how to use /v1/estimate_gas_price json-rpc in GetBlock.io Web3 documentation.
---

# /v1/estimate\_gas\_price - Aptos

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://apt.getblock.io/v1/estimate_gas_price?' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
```

#### Response

```java
{
    "deprioritized_gas_estimate": 100,
    "gas_estimate": 100,
    "prioritized_gas_estimate": 150
}
```
