---
description: >-
  Example code for the /transaction/fee-estimate REST method. Complete guide on
  how to use /transaction/fee-estimate REST in GetBlock Web3 documentation.
---

# /transaction/fee-estimate - Polkadot

This endpoint estimates the fee and weight for a submitted transaction without broadcasting it.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/transaction/fee-estimate`).
{% endhint %}

## Endpoint

```
POST /transaction/fee-estimate
```

## Body Parameters

| Field | Type   | Required | Description                              |
| ----- | ------ | -------- | ---------------------------------------- |
| tx    | string | Yes      | A SCALE-encoded transaction, hex-encoded |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/transaction/fee-estimate' \
--header 'Content-Type: application/json' \
--data-raw '{"tx": "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/transaction/fee-estimate', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
  "tx": "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."
})
});
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/transaction/fee-estimate',
    json={
  "tx": "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."
}
)
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "weight": {
    "refTime": "133700000",
    "proofSize": "3593"
  },
  "class": "Normal",
  "partialFee": "15960000000"
}
```

## Response Fields

| Field      | Type   | Description                             |
| ---------- | ------ | --------------------------------------- |
| weight     | object | Dispatch weight (refTime and proofSize) |
| class      | string | Dispatch class                          |
| partialFee | string | Estimated fee in Planck, excluding tip  |

## Use Cases

* **Fee Preview**: Show the estimated fee before signing
* **Budgeting**: Estimate fees for batches of transactions
* **Wallet UX**: Display expected costs to users
* **Automation**: Gate submissions on fee thresholds
