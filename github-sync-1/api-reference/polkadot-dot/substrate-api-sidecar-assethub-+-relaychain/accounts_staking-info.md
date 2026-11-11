---
description: >-
  Example code for the /accounts/{accountId}/staking-info REST method. Complete
  guide on how to use /accounts/{accountId}/staking-info REST in GetBlock Web3
  documentation.
---

# /accounts/{accountId}/staking-info - Polkadot

This endpoint returns the staking information for a stash account, including the controller, reward destination, and the bonded, active, and unlocking amounts.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/accounts/{accountId}/staking-info`).
{% endhint %}

## Endpoint

```http
GET /accounts/{accountId}/staking-info
```

## Path Parameters

| Parameter | Type   | Description                |
| --------- | ------ | -------------------------- |
| accountId | string | SS58 stash account address |

## Query Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| at        | string | No       | Block height or hash to query at |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/staking-info'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/staking-info');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/staking-info')
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
  "controller": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5",
  "rewardDestination": "Staked",
  "numSlashingSpans": "0",
  "staking": {
    "stash": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5",
    "total": "100000000000",
    "active": "100000000000",
    "unlocking": [],
    "claimedRewards": [
      "1400",
      "1401"
    ]
  }
}
```

## Response Fields

| Field             | Type   | Description                      |
| ----------------- | ------ | -------------------------------- |
| controller        | string | Controller account for the stash |
| rewardDestination | string | Where staking rewards are sent   |
| staking.total     | string | Total bonded amount in Planck    |
| staking.active    | string | Actively bonded amount in Planck |
| staking.unlocking | array  | Amounts currently unbonding      |

## Use Cases

* **Staking Dashboards**: Show a nominator's bonded and active stake
* **Reward Tracking**: Read claimed reward eras
* **Unbonding UX**: Display amounts in the unbonding period
* **Portfolio Tools**: Aggregate staking positions
