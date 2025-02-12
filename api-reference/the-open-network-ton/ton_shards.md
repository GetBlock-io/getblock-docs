---
description: >-
  Example code for the /shards json-rpc method. Сomplete guide on how to use
  /shards json-rpc in GetBlock.io Web3 documentation.
---

# /shards - The Open Network (TON)

#### Parameters

`seqno` - query

required, integer

Masterchain seqno to fetch shards of.

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/shards?seqno=30497145' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "@extra": "1687338545.963773:2:0.6247463132596943",
        "@type": "blocks.shards",
        "shards": [
            {
                "@type": "ton.blockIdExt",
                "file_hash": "7+lqLAwGnSkAucY1bbf/79LwKdtEjrCd3BvtkN8NzyQ=",
                "root_hash": "TqVSpe+5Dxx+RnBdvJ/tJeg13/nAAh9mjR/C+Jy4mmM=",
                "seqno": 36175355,
                "shard": "-9223372036854775808",
                "workchain": 0
            }
        ]
    }
}
```
