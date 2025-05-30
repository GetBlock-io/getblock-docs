---
description: >-
  Example code for the eth_subscribe json-rpc method. Сomplete guide on how to
  use eth_subscribe json-rpc in GetBlock.io Web3 documentation.
---

# eth\_subscribe - Gnosis

#### Parameters

`type` - string

A subscription type, such as newHeads (new headers appended to the chain, including chain reorganizations), logs (logs that are included in new imported blocks and match the given filter criteria) or newPendingTransactions (hashes for all transactions that are added to the pending state and are signed with a key that is available in the node).

`objects` - hex string

Optional argumants such as an address, multiple addresses, and topics.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_subscribe",
"params": ["newHeads", null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32600,
        "message": "eth_subscribe found for the url 'http://0.0.0.0:8545' but is disabled for Https"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
