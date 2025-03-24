---
description: >-
  Example code for the babe_epochAuthorship  {disallowed} json-rpc method.
  Сomplete guide on how to use babe_epochAuthorship  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# babe\_epochAuthorship {disallowed} - Kusama

#### Parameters

\-

#### Request

<pre class="language-java"><code class="lang-java"><strong>curl --location --request POST 'https://go.getblock.io/&#x3C;ACCESS-TOKEN>/' \
</strong>--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "babe_epochAuthorship",
"params": [],
"id": "getblock.io"}'
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
