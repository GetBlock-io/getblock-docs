# node\_transaction pool

This endpoint returns the extrinsics currently in the node's transaction pool.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/node/transaction-pool`).
{% endhint %}

## Endpoint

```
GET /node/transaction-pool
```

## Query Parameters

| Parameter  | Type    | Required | Description                                       |
| ---------- | ------- | -------- | ------------------------------------------------- |
| includeFee | boolean | No       | Include the partial fee for each pooled extrinsic |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/node/transaction-pool'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/node/transaction-pool');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/node/transaction-pool')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "pool": [
    {
      "hash": "0x8e6c1c...",
      "encodedExtrinsic": "0x4d0284..."
    }
  ]
}
```

## Response Fields

| Field                    | Type   | Description                          |
| ------------------------ | ------ | ------------------------------------ |
| pool                     | array  | Pending extrinsics in the pool       |
| pool\[].hash             | string | Extrinsic hash                       |
| pool\[].encodedExtrinsic | string | SCALE-encoded extrinsic, hex-encoded |

## Use Cases

* **Mempool Views**: Inspect pending transactions
* **Fee Markets**: Observe pending demand for block space
* **Resubmission**: Detect whether a transaction is still pending
* **Monitoring**: Track pool depth
