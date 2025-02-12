---
description: >-
  Example code for the /lookupBlock json-rpc method. Сomplete guide on how to
  use /lookupBlock json-rpc in GetBlock.io Web3 documentation.
---

# /lookupBlock - The Open Network (TON)

#### Parameters

`workchain` - query

required, integer

block workchain id

`shard` - query

required, integer

block shard id

`seqno` - query

optional, integer

block height

`lt` - query

optional, integer

block's Logical time

`unixtime` - query

optional, integer

block's unix time

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/lookupBlock?workchain=-1&shard=-9223372036854775808&seqno=30497145' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "@extra": "1687338545.5527961:2:0.1486653485640057",
        "@type": "ton.blockIdExt",
        "file_hash": "t81TFPL17RLgqteZsYf64yY2EX18p/V5f3Z9itK1DrA=",
        "root_hash": "2a8ZfAc+YN+hWDxtnlj+lsmPxDljA1ur8XVvL52UB2s=",
        "seqno": 30497145,
        "shard": "-9223372036854775808",
        "workchain": -1
    }
}
```
