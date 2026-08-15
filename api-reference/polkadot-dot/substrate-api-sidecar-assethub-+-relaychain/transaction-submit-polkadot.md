---
description: >-
  Example code for the /transaction REST method. Complete guide on how to use
  /transaction REST in GetBlock Web3 documentation.
---

# /transaction - Polkadot

This endpoint submits a signed, SCALE-encoded transaction to the network and returns its hash.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/transaction`).
{% endhint %}

## Endpoint

```http
POST /transaction
```

## Body Parameters

| Field | Type   | Required | Description                                      |
| ----- | ------ | -------- | ------------------------------------------------ |
| tx    | string | Yes      | A signed, SCALE-encoded transaction, hex-encoded |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/transaction' \
--header 'Content-Type: application/json' \
--data-raw '{"tx": "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/transaction', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/transaction',
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
  "hash": "0x8e6c1c623b9d2a2c0e6f6c3b8f0a5d5d1a4b8c9e0f1a2b3c4d5e6f7a8b9c0d1e"
}
```

## Response Fields

| Field | Type   | Description                           |
| ----- | ------ | ------------------------------------- |
| hash  | string | The hash of the submitted transaction |

## Use Cases

* **Transaction Broadcast**: Submit a signed transaction over REST
* **Backends**: Broadcast from a server without a JSON-RPC client
* **Custody Flows**: Submit externally signed transactions
* **Automation**: Broadcast scheduled transactions
