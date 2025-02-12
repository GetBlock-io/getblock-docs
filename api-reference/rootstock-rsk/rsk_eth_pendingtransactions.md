---
description: >-
  Example code for the eth_pendingTransactions json-rpc method. Сomplete guide
  on how to use eth_pendingTransactions json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_pendingTransactions - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://rsk.getblock.io/YOUR-API-KEY/mainnet/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_pendingTransactions",
"params": [],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        {
            "blockHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "blockNumber": null,
            "from": "0x28bdb9c230f4d5e45435e4d006326ee32e487b31",
            "gas": "0x205940",
            "gasPrice": "0x4a817c800",
            "hash": "0x8e4340ea3983d86e4b6c44249362f716ec9e09849ef9b6e3321140581d2e4dac",
            "input": "0xe4b6c4424936",
            "nonce": "0x14",
            "to": null,
            "transactionIndex": "0x0",
            "value": "0x0",
            "v": "0x3d",
            "r": "0x1ec191ef20b0e9628c4397665977cbe7a53a263c04f6f185132b77fa0fd5ca44",
            "s": "0x8a58e00c63e05cfeae4f1cf19f05ce82079dc4d5857e2cc281b7797d58b5faf"
        }
    ]
}
```
