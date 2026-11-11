---
description: >-
  Example code for the api/v2/sendtx REST method. Complete guide on how to use
  the api/v2/sendtx REST method in the GetBlock Web3 documentation.
---

# api/v2/sendtx - Zcash

This endpoint broadcasts a signed, serialized transaction to the Zcash network through the Zebra node and returns its transaction id on acceptance. Transparent and shielded transactions are both accepted.

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
--data-raw '050000800a27a726...'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/',
    { method: 'POST', headers: { 'Content-Type': 'text/plain' }, body: '050000800a27a726...' }
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
    data='050000800a27a726...'
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "result": "076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37"
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| result | string | Transaction id of the accepted transaction |

{% hint style="warning" %}
Acceptance means the node admitted the transaction to its mempool, not that it has been mined. A Zcash transaction also carries an `expiryheight`, after which it can no longer be mined; if it has not confirmed by then, it must be rebuilt rather than rebroadcast.

The fee must meet the ZIP-317 conventional fee of 5,000 zatoshis per logical action, with a minimum of two actions. Blockbook's `estimatefee` endpoint is not available on Zcash, so compute the fee from that rule. The transaction must also be signed for the consensus branch ID in force, reported under `backend.consensus` by [api/status](api-status-zcash.md).
{% endhint %}

## Use Cases

* **Wallet Sends**: Broadcast a transaction built and signed on the client
* **Payment Settlement**: Push a payout to the network
* **Shielding**: Broadcast a transaction moving transparent funds into a shielded pool

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The hex is malformed or not a complete signed transaction |
| 400 | Transaction rejected | The fee is below the ZIP-317 minimum, the expiry height has passed, or the branch ID is wrong |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 500 | Internal error | The node failed to process the broadcast |
