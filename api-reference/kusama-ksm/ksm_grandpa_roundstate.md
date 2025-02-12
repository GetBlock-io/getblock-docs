---
description: >-
  Example code for the grandpa_roundState json-rpc method. Сomplete guide on how
  to use grandpa_roundState json-rpc in GetBlock.io Web3 documentation.
---

# grandpa\_roundState - Kusama

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://ksm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "grandpa_roundState",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "background": [],
        "best": {
            "precommits": {
                "currentWeight": 0,
                "missing": [
                    "12cCLNxo2WqvqizuMrgHtpjUz2s5xmomTM7y3FWESbvjcwq",
                    "16mnvMThaT5GkGrCktQgGdwfoSqx1zEzVkMAABTbGGX8Hn2U",
                    "16nbm1KzDpQKx8CfDcaXW6Mb9HbLbUKRo34Vsjq5SYnWNUwB"
                ]
            },
            "prevotes": {
                "currentWeight": 0,
                "missing": [
                    "12cCLNxo2WqvqizuMrgHtpjUz2s5xmomTM7y3FWESbvjcwq",
                    "14ZryeDT1MmSSX29QaJ2pbNjJDv8hoLTfwFmDwe6mDMh3Vj",
                    "15E5zYJhV2iqZXmL1FKe8MdFocj9n1phEaJob4izAaXA9bw",
                    "16nbm1KzDpQKx8CfDcaXW6Mb9HbLbUKRo34Vsjq5SYnWNUwB"
                ]
            },
            "round": 17242,
            "thresholdWeight": 199,
            "totalWeight": 297
        },
        "setId": 1477
    }
}
```
