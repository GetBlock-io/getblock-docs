---
description: >-
  Example code for the /getAddressInformation json-rpc method. Сomplete guide on
  how to use /getAddressInformation json-rpc in GetBlock.io Web3 documentation.
---

# /getAddressInformation - The Open Network (TON)

#### Parameters

`address` - query

required, string

Identifier of target TON account in any form.

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/getAddressInformation?address=EQAFPsHznqgqPUZQVyUBmWxpNwReAIuepKo_BjLiS-C05XB_' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "@extra": "1687338513.0129201:2:0.9501899250701279",
        "@type": "raw.fullAccountState",
        "balance": "809401535972",
        "block_id": {
            "@type": "ton.blockIdExt",
            "file_hash": "s6I65ivzwVkCLTCTVH0dIS7321rUFfx+4udx/9RRZB8=",
            "root_hash": "lV9RIaO7dQ01pfJhbzR5POMQ9uHgSqAAgcPpy4/K6qI=",
            "seqno": 30517690,
            "shard": "-9223372036854775808",
            "workchain": -1
        },
        "code": "te6cckEBAQEAcQAA3v8AIN0gggFMl7ohggEznLqxn3Gw7UTQ0x/THzHXC//jBOCk8mCDCNcYINMf0x/TH/gjE7vyY+1E0NMf0x/T/9FRMrryoVFEuvKiBPkBVBBV+RDyo/gAkyDXSpbTB9QC+wDo0QGkyMsfyx/L/8ntVBC9ba0=",
        "data": "te6cckEBAQEAKgAAUAAAAAYpqaMXUl8jsumwVsbvK5FBr1eiVrHpUrHZ8HRDfRo1lFx8lpd765yB",
        "frozen_hash": "",
        "last_transaction_id": {
            "@type": "internal.transactionId",
            "hash": "R7vI2AxqgpQQYxQaxZsQwtZXHxE6rw6mDNtKJ3LtPVM=",
            "lt": "34789408000007"
        },
        "state": "active",
        "sync_utime": 1687338490
    }
}
```
