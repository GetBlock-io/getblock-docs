---
description: >-
  Example code for the /getWalletInformation json-rpc method. Сomplete guide on
  how to use /getWalletInformation json-rpc in GetBlock.io Web3 documentation.
---

# /getWalletInformation - The Open Network (TON)

#### Parameters

`address` - query

required, string

Identifier of target TON account in any form.

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/getWalletInformation?address=EQAFPsHznqgqPUZQVyUBmWxpNwReAIuepKo_BjLiS-C05XB_' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "account_state": "active",
        "balance": "809401535972",
        "last_transaction_id": {
            "@type": "internal.transactionId",
            "hash": "R7vI2AxqgpQQYxQaxZsQwtZXHxE6rw6mDNtKJ3LtPVM=",
            "lt": "34789408000007"
        },
        "seqno": 6,
        "wallet": true,
        "wallet_id": 698983191,
        "wallet_type": "wallet v3 r2"
    }
}
```
