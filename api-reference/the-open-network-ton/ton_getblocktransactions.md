---
description: >-
  Example code for the /getBlockTransactions json-rpc method. Сomplete guide on
  how to use /getBlockTransactions json-rpc in GetBlock.io Web3 documentation.
---

# /getBlockTransactions - The Open Network (TON)

#### Parameters

`workchain` - query

required, integer

block workchain id

`shard` - query

required, integer

block shard id

`seqno` - query

required, integer

block seqno

`root_hash` - query

optional, string

`file_hash` - query

optional, string

`after_lt` - query

optional, integer

`after_hash` - query

optional, string

`count` - query

optional, integer, default 40

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/getBlockTransactions?workchain=-1&shard=-9223372036854775808&seqno=30497145&root_hash=TqVSpe+5Dxx+RnBdvJ/tJeg13/nAAh9mjR/C+Jy4mmM=&count=5' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "@extra": "1687338546.391785:11:0.37351974452237247",
        "@type": "blocks.transactions",
        "id": {
            "@type": "ton.blockIdExt",
            "file_hash": "t81TFPL17RLgqteZsYf64yY2EX18p/V5f3Z9itK1DrA=",
            "root_hash": "2a8ZfAc+YN+hWDxtnlj+lsmPxDljA1ur8XVvL52UB2s=",
            "seqno": 30497145,
            "shard": "-9223372036854775808",
            "workchain": -1
        },
        "incomplete": false,
        "req_count": 5,
        "transactions": [
            {
                "@type": "blocks.shortTxId",
                "account": "-1:3333333333333333333333333333333333333333333333333333333333333333",
                "hash": "qBth3wIytHqavKianhcpmnFlwHIF5TXV3nXZPgjJ9Dc=",
                "lt": "38636875000001",
                "mode": 135
            },
            {
                "@type": "blocks.shortTxId",
                "account": "-1:3333333333333333333333333333333333333333333333333333333333333333",
                "hash": "w0de1XEy/dYChgNYy0w099SltwGrVvP4gf372c+RUZo=",
                "lt": "38636875000002",
                "mode": 135
            },
            {
                "@type": "blocks.shortTxId",
                "account": "-1:5555555555555555555555555555555555555555555555555555555555555555",
                "hash": "5I6TUoKfhtpaqFWzEfZ3v1HUd1fVLPORb8gVRSuqsGQ=",
                "lt": "38636875000003",
                "mode": 135
            }
        ]
    }
}
```
