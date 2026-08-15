---
description: >-
  Example code for the /pallets/{palletId}/consts REST method. Complete guide on
  how to use /pallets/{palletId}/consts REST in GetBlock Web3 documentation.
---

# /pallets/{palletId}/consts - Polkadot

This endpoint returns the constants defined by a pallet, such as bonding durations or deposit amounts.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/pallets/{palletId}/consts`).
{% endhint %}

## Endpoint

```http
GET /pallets/{palletId}/consts
```

## Path Parameters

| Parameter | Type   | Description          |
| --------- | ------ | -------------------- |
| palletId  | string | Pallet name or index |

## Query Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| at        | string | No       | Block height or hash to query at |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/pallets/staking/consts'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/pallets/staking/consts');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/pallets/staking/consts')
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
  "pallet": "staking",
  "palletIndex": "7",
  "items": [
    {
      "name": "BondingDuration",
      "type": "u32",
      "value": "28"
    },
    {
      "name": "MaxNominations",
      "type": "u32",
      "value": "16"
    }
  ]
}
```

## Response Fields

| Field          | Type   | Description                     |
| -------------- | ------ | ------------------------------- |
| pallet         | string | Pallet name                     |
| items          | array  | Constants defined by the pallet |
| items\[].name  | string | Constant name                   |
| items\[].value | string | Constant value                  |

## Use Cases

* **Parameter Reads**: Read a pallet's on-chain constants
* **Staking UX**: Show the bonding duration to users
* **Governance Tools**: Read deposit and threshold constants
* **Documentation**: Surface runtime parameters
