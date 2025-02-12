---
description: >-
  Example code for the /getConsensusBlock json-rpc method. Сomplete guide on how
  to use /getConsensusBlock json-rpc in GetBlock.io Web3 documentation.
---

# /getConsensusBlock - The Open Network (TON)

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/getConsensusBlock' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "consensus_block": 30517698,
        "timestamp": 1687338534.241151
    }
}
```
