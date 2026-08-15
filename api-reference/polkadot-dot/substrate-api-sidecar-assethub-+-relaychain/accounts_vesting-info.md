# accounts\_vesting info

This endpoint returns the vesting schedules for an account, including the locked amount, the per-block unlock, and the starting block of each schedule.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/accounts/{accountId}/vesting-info`).
{% endhint %}

## Endpoint

```
GET /accounts/{accountId}/vesting-info
```

## Path Parameters

| Parameter | Type   | Description          |
| --------- | ------ | -------------------- |
| accountId | string | SS58 account address |

## Query Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| at        | string | No       | Block height or hash to query at |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/vesting-info'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/vesting-info');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/vesting-info')
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
  "vesting": [
    {
      "locked": "10000000000",
      "perBlock": "1000000",
      "startingBlock": "6000000"
    }
  ]
}
```

## Response Fields

| Field                    | Type   | Description                          |
| ------------------------ | ------ | ------------------------------------ |
| vesting                  | array  | Vesting schedules for the account    |
| vesting\[].locked        | string | Total locked amount in Planck        |
| vesting\[].perBlock      | string | Amount unlocked per block, in Planck |
| vesting\[].startingBlock | string | Block at which vesting starts        |

## Use Cases

* **Vesting UX**: Show a user their unlock schedule
* **Treasury Tools**: Track vested distributions
* **Compliance**: Report locked allocations
* **Forecasting**: Project future unlocks
