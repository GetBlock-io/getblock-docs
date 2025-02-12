---
description: >-
  Example code for the /getMasterchainBlockSignatures json-rpc method. Сomplete
  guide on how to use /getMasterchainBlockSignatures json-rpc in GetBlock.io
  Web3 documentation.
---

# /getMasterchainBlockSignatures - The Open Network (TON)

#### Parameters

`seqno` - query

required, integer

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet/getMasterchainBlockSignatures?seqno=30497145' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "@extra": "1687338543.6958232:6:0.19692230851537695",
        "@type": "blocks.blockSignatures",
        "id": {
            "@type": "ton.blockIdExt",
            "file_hash": "t81TFPL17RLgqteZsYf64yY2EX18p/V5f3Z9itK1DrA=",
            "root_hash": "2a8ZfAc+YN+hWDxtnlj+lsmPxDljA1ur8XVvL52UB2s=",
            "seqno": 30497145,
            "shard": "-9223372036854775808",
            "workchain": -1
        },
        "signatures": [
            {
                "@type": "blocks.signature",
                "node_id_short": "/sZBm51kgjzTPFF6yRy3Es2U4UTw8Wl1vOSaTaGBQUY=",
                "signature": "vVWT6U7M3nqrRr+zfmtg5TNI2A82Hdu8VMTej3P/bI4pMbRrQ8QHs57BrK6KeYes3vOyShMAPPCQZMSGPLbBDg=="
            },
            {
                "@type": "blocks.signature",
                "node_id_short": "3Gtnxj1TpScX0DLnUs3G6sNqsuZt8AVA/870EyMPAE8=",
                "signature": "LBpEGy21IyiJ84SIPWGEAkp2VIhkyILwe9RR4WVhtiedC5apiZyTK+f2pTGNoylD5NCpo6dCywF+jM+0qDkMCQ=="
            },
            {
                "@type": "blocks.signature",
                "node_id_short": "wTXXj2aLhQvJxpy6KqiBL/xvbIMtQ5loLBVO8Z9q9Ow=",
                "signature": "fGSCMmxb6FbsqXAJ04/E79eKBvvSZ5crvERYnDHr4/HkK4/Y9MVfCGz88GSSxJBXvtkDNJY/sD6ZQU84VMFlBw=="
            }
        ]
    }
}
```
