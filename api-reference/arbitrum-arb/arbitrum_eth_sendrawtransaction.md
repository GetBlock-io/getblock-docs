---
description: >-
  Example code for the eth_sendRawTransaction json-rpc method. Сomplete guide on
  how to use eth_sendRawTransaction json-rpc in GetBlock.io Web3 documentation.
---

# eth\_sendRawTransaction - Arbitrum

#### Parameters

`DATA` - string

The signed transaction data.

#### Request

<pre class="language-java"><code class="lang-java">curl --location --request POST 'https://go.getblock.io/YOUR-ACCESS-TOKEN' \
<strong>--header 'Content-Type: application/json' \
</strong>--data-raw '{"jsonrpc": "2.0", "method": "eth_sendRawTransaction", "params": ["0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"], "id": "getblock.io"}'
</code></pre>

#### Response

```java
{
    "error": {
        "code": -32000,
        "message": "rlp: element is larger than containing list"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
