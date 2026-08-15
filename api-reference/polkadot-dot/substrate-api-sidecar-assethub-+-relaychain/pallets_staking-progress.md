# pallets\_staking progress

This endpoint returns the current staking system status: the active era, session progress, and the timing of the next era and unapplied slashes.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/pallets/staking/progress`).
{% endhint %}

## Endpoint

```http
GET /pallets/staking/progress
```

## Query Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| at        | string | No       | Block height or hash to query at |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/pallets/staking/progress'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/pallets/staking/progress');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/pallets/staking/progress')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "at": {
    "hash": "0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32",
    "height": "6754362"
  },
  "idealValidatorCount": "600",
  "activeEra": "1400",
  "forceEra": "NotForcing",
  "nextActiveEraEstimate": "6760000",
  "electionStatus": {
    "status": {
      "close": null
    }
  },
  "unappliedSlashes": []
}
```

## Response Fields

| Field                 | Type   | Description                     |
| --------------------- | ------ | ------------------------------- |
| activeEra             | string | Current active staking era      |
| idealValidatorCount   | string | Target number of validators     |
| nextActiveEraEstimate | string | Estimated block of the next era |
| unappliedSlashes      | array  | Slashes not yet applied         |

## Use Cases

* **Staking Dashboards**: Show era progress and validator count
* **Reward Timing**: Estimate when the next era starts
* **Validator Tools**: Monitor the election status
* **Alerting**: Watch for unapplied slashes
