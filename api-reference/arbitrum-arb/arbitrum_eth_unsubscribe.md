---
description: >-
  Example code for the eth_unsubscribe json-rpc method. Сomplete guide on how to
  use eth_unsubscribe json-rpc in GetBlock.io Web3 documentation.
---

# eth\_unsubscribe - Arbitrum

#### Parameters

`data` - hex string

A subscription ID previously generated with `eth_subscribe` method.

#### Request



{% tabs %}
{% tab title="ws" %}
```java
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/ -x '{ 
  "jsonrpc": "2.0", 
  "id": "getblock.io", 
  "method": "eth_unsubscribe", 
  "params": ["<SUBSCRIPTION_ID>"] 
}'
```
{% endtab %}

{% tab title="curl" %}
```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_unsubscribe",
  "params": ["0xe5af64ddfd365b4632988c5935cfedb7"],
  "id": "getblock.io"
}'
```
{% endtab %}
{% endtabs %}

#### Response

```java
{
    "jsonrpc":"2.0",
    "id":"getblock.io",
    "result":true
}
```
