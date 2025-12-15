---
description: >-
  Example code for the shh_hasIdentity JSON RPC method. Сomplete guide on how to
  use shh_hasIdentity JSON RPC in GetBlock Web3 documentation.
---

# shh\_hasIdentity {disallowed} - Arbitrum

#### Parameters

`address` - data

the address of the new identiy.

#### Request

<pre class="language-java"><code class="lang-java">curl --location --request POST 'https://go.getblock.io/&#x3C;ACCESS-TOKEN>' \
<strong>--header 'Content-Type: application/json' \
</strong>--data-raw '{
  "jsonrpc": "2.0",
  "method": "shh_hasIdentity",
  "params": [null],
  "id": "getblock.io"
}'
</code></pre>

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
