---
description: >-
  Example code for the api/v2/sendtx REST method. Complete guide on how to use
  the api/v2/sendtx REST method in the GetBlock Web3 documentation.
---

# api/v2/sendtx - Litecoin

This endpoint broadcasts a signed, serialized transaction to the Litecoin network through the backend node and returns its transaction id on acceptance.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| hex | string | body | Yes | Hex-encoded signed raw transaction, sent as the plain-text request body |

## Request

{% hint style="warning" %}
The trailing slash on `/api/v2/sendtx/` is mandatory. Request bodies are limited to 8 MiB.
{% endhint %}

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/' \
--header 'Content-Type: text/plain' \
--data-raw '0200000001...'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/',
    { method: 'POST', headers: { 'Content-Type': 'text/plain' }, body: '0200000001...' }
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/',
    headers={'Content-Type': 'text/plain'},
    data='0200000001...'
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "result": "51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969"
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| result | string | Transaction id of the accepted transaction |

{% hint style="warning" %}
Acceptance means the node admitted the transaction to its mempool, not that it has been mined. Track it to the required depth with [api/v2/tx](api-v2-tx-litecoin.md) or a [subscribeAddresses](../litecoin-blockbook-websocket-api/subscribeaddresses-litecoin.md) subscription before treating a payment as settled.
{% endhint %}

## Use Cases

* **Wallet Sends**: Broadcast a transaction built and signed on the client
* **Payment Settlement**: Push a payout to the network
* **Consolidation**: Broadcast a transaction combining several small outputs
* **Retry Flows**: Rebroadcast a transaction that has not been mined

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The hex is malformed or not a complete signed transaction |
| 400 | min relay fee not met | The fee is below the node's relay threshold |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 500 | Internal error | The node rejected or failed to process the broadcast |
