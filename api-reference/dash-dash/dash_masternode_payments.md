---
description: >-
  Example code for the masternode_payments json-rpc method. Сomplete guide on
  how to use masternode_payments json-rpc in GetBlock.io Web3 documentation.
---

# masternode\_payments - Dash

#### Parameters

`method name` - string

None

`Block Hash` - string (hex)

Optional

The hash of the starting block (default: tip)

`count` - number (int)

Optional

The number of blocks to return (default: 1). Will return previous blocks if is negative. Both 1 and -1 correspond to the chain tip.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "masternode",
"params": ["payments", null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        {
            "height": 407822,
            "blockhash": "0000030ae0ca262af12918eac9069e61c481e17ac65a26c87ee44427699c3f3a",
            "amount": 1253571429,
            "masternodes": [
                {
                    "proTxHash": "c865b48a09801c61dce5804f28fe994c72577254ea1859cf1c37fe92b428e757",
                    "amount": 1253571429,
                    "payees": [
                        {
                            "address": "yP72QU7PMG8wNQVTtaQrCLVKCmbuDeAK91",
                            "script": "76a9141e8efb321d5cad77e28e4e6b51546932579d02f588ac",
                            "amount": 1253571429
                        }
                    ]
                }
            ]
        }
    ]
}
```
