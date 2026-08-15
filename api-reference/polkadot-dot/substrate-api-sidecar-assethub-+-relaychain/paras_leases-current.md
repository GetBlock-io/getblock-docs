# paras\_leases current

This endpoint returns the current parachain lease period and the parachains that currently hold a lease. It is a relay-chain endpoint.

## Endpoint

```http
GET /paras/leases/current
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
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/paras/leases/current'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/paras/leases/current');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/paras/leases/current')
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
  "leasePeriodIndex": "42",
  "endOfLeasePeriod": "6800000",
  "currentLeaseHolders": [
    "2004",
    "2006"
  ]
}
```

## Response Fields

| Field               | Type   | Description                                  |
| ------------------- | ------ | -------------------------------------------- |
| leasePeriodIndex    | string | Current lease period index                   |
| endOfLeasePeriod    | string | Block at which the current lease period ends |
| currentLeaseHolders | array  | Parachain IDs currently holding a lease      |

## Use Cases

* **Slot Tracking**: Track which parachains hold slots
* **Lease Expiry**: Read when the current lease period ends
* **Ecosystem Tools**: Monitor slot occupancy
* **Analytics**: Study parachain slot tenure
