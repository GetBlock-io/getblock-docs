---
description: >-
  Example code for the eth_newFilter json-rpc method. Сomplete guide on how to
  use eth_newFilter json-rpc in GetBlock.io Web3 documentation.
---

# eth\_newFilter - Arbitrum

#### Parameters

`Object` - object

Filter options

#### Request

<pre class="language-java"><code class="lang-java">curl --location --request POST 'https://go.getblock.io/YOUR-ACCESS-TOKEN' \
<strong>--header 'Content-Type: application/json' \
</strong>--data-raw '{"jsonrpc": "2.0", "method": "eth_newFilter", "params": [{"fromBlock": "earliest", "toBlock": "latest", "topics": []}], "id": "getblock.io"}'
</code></pre>

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x7a624b4e4688bb09c14e858427d9ede5"
}
```
