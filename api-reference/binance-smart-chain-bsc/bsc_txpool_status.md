---
description: >-
  Example code for the txpool_status json-rpc method. Сomplete guide on how to
  use txpool_status json-rpc in GetBlock.io Web3 documentation.
---

# txpool\_status - Binance Smart Chain

#### Parameters

\-

#### Request

<pre class="language-java"><code class="lang-java"><strong>curl --location --request POST 'https://go.getblock.io/&#x3C;ACCESS-TOKEN>/v1/mainnet/' \
</strong>--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "txpool_status",
"params": [],
"id": "getblock.io"}'
</code></pre>

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "pending": "0xf7",
        "queued": "0x112"
    }
}
```
