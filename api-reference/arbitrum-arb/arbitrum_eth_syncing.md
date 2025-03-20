---
description: >-
  Example code for the eth_syncing json-rpc method. Сomplete guide on how to use
  eth_syncing json-rpc in GetBlock.io Web3 documentation.
---

# eth\_syncing - Arbitrum

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/arbitrum/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_syncing",
  "params": [],
  "id": "getblock.io"
}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": false
}
```
