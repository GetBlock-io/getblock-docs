---
description: >-
  Example code for the /getMasterchainInfo json-rpc method. Сomplete guide on
  how to use /getMasterchainInfo json-rpc in GetBlock.io Web3 documentation.
---

# /getMasterchainInfo - The Open Network (TON)

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/getMasterchainInfo' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "@extra": "1687338543.4110794:14:0.08925892221264009",
        "@type": "blocks.masterchainInfo",
        "init": {
            "@type": "ton.blockIdExt",
            "file_hash": "XplPz01CXAps5qeSWUtxcyBfdAo5zVb1N979KLSKD24=",
            "root_hash": "F6OpKZKqvqeFp6CQmFomXNMfMj2EnaUSOXN+Mh+wVWk=",
            "seqno": 0,
            "shard": "0",
            "workchain": -1
        },
        "last": {
            "@type": "ton.blockIdExt",
            "file_hash": "yhFiArzGTVG04DNU0L+usdfOCwmpEFCjvouHKUluZ0I=",
            "root_hash": "SV/NmAOmNTzjkuJO9uyroF7jv0KEiJyNFd34Ye5AZsY=",
            "seqno": 30517698,
            "shard": "-9223372036854775808",
            "workchain": -1
        },
        "state_root_hash": "SrlVEK6MEz8sUXG0+JKXIwBGz9lx79NPwxKatePa0II="
    }
}
```
