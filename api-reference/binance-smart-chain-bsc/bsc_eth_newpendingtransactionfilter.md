---
description: >-
  Example code for the eth_newPendingTransactionFilter json-rpc method. Сomplete
  guide on how to use eth_newPendingTransactionFilter json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_newPendingTransactionFilter - Binance Smart Chain

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_newPendingTransactionFilter",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0xe4e164bfaa484b9b998772b6dfe9c7cd"
}
```
