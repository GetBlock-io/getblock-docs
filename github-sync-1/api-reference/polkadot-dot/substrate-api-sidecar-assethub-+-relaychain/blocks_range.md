---
description: >-
  Example code for the /blocks REST method. Complete guide on how to use /blocks
  REST in GetBlock Web3 documentation.
---

# /blocks - Polkadot

This endpoint returns a range of blocks in a single call, decoded like /blocks/{blockId}. The range is limited to keep responses bounded.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/blocks`).
{% endhint %}

## Endpoint

```
GET /blocks
```

## Query Parameters

| Parameter | Type   | Required | Description                               |
| --------- | ------ | -------- | ----------------------------------------- |
| range     | string | Yes      | Inclusive block-number range, such as 0-1 |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/blocks?range=6754360-6754362'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/blocks?range=6754360-6754362');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/blocks?range=6754360-6754362')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
  {
    "number": "6754360",
    "hash": "0xabc...",
    "extrinsics": []
  },
  {
    "number": "6754361",
    "hash": "0xdef...",
    "extrinsics": []
  }
]
```

## Response Fields

| Field          | Type   | Description                                                |
| -------------- | ------ | ---------------------------------------------------------- |
| \[]            | array  | Array of decoded block objects, one per block in the range |
| \[].number     | string | Block number                                               |
| \[].hash       | string | Block hash                                                 |
| \[].extrinsics | array  | Decoded extrinsics for the block                           |

## Use Cases

* **Batch Indexing**: Ingest several blocks in one request
* **Backfilling**: Backfill a historical range efficiently
* **Analytics**: Scan a window of blocks for activity
* **Reduced Round-Trips**: Avoid per-block requests
