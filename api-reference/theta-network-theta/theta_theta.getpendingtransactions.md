---
description: >-
  Example code for the theta.GetPendingTransactions json-rpc method. Сomplete
  guide on how to use theta.GetPendingTransactions json-rpc in GetBlock.io Web3
  documentation.
---

# theta.GetPendingTransactions - Theta Network

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "theta.GetPendingTransactions",
"params": {},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "tx_hashes": [
            "0x61ed06b78fededbbd262f95f321d7e48dee81e9b1e493b7f4d42c6bf7afd4b27",
            "0xc4162541f5e9f283bd9c3beb2798a4a2539b567dd35f52edefde7063f985ab17"
        ]
    }
}
```
