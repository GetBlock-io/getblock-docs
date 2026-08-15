# pallets\_nomination pool

This endpoint returns the details of a nomination pool: its state, points, member count, and bonded account.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/pallets/nomination-pools/{poolId}`).
{% endhint %}

## Endpoint

```
GET /pallets/nomination-pools/{poolId}
```

## Path Parameters

| Parameter | Type   | Description            |
| --------- | ------ | ---------------------- |
| poolId    | string | The nomination pool ID |

## Query Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| at        | string | No       | Block height or hash to query at |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/pallets/nomination-pools/12'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/pallets/nomination-pools/12');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/pallets/nomination-pools/12')
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
  "poolId": "12",
  "state": "Open",
  "points": "500000000000",
  "memberCounter": "320",
  "roles": {
    "depositor": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5",
    "root": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5"
  }
}
```

## Response Fields

| Field         | Type   | Description                            |
| ------------- | ------ | -------------------------------------- |
| poolId        | string | The nomination pool ID                 |
| state         | string | Pool state (Open, Blocked, Destroying) |
| points        | string | Total points bonded in the pool        |
| memberCounter | string | Number of pool members                 |

## Use Cases

* **Pool Staking UX**: Show pool state and size to users
* **Yield Tools**: Compare pools by bonded stake
* **Membership**: Read a pool's member count
* **Monitoring**: Track pool health
