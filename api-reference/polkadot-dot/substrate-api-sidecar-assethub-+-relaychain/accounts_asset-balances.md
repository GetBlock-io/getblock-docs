# accounts\_asset balances

This endpoint returns an account's balances for Asset Hub assets. Specific asset IDs can be requested with the assets query parameter.

{% hint style="info" %}
This endpoint is specific to Asset Hub (assets, foreign assets, and NFTs) and is served on the default network.
{% endhint %}

## Endpoint

```
GET /accounts/{accountId}/asset-balances
```

## Path Parameters

| Parameter | Type   | Description          |
| --------- | ------ | -------------------- |
| accountId | string | SS58 account address |

## Query Parameters

| Parameter | Type   | Required | Description                                              |
| --------- | ------ | -------- | -------------------------------------------------------- |
| assets\[] | array  | No       | Asset IDs to query. Returns all held assets when omitted |
| at        | string | No       | Block height or hash to query at                         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/asset-balances?assets[]=1984'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/asset-balances?assets[]=1984');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/accounts/15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5/asset-balances?assets[]=1984')
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
  "assets": [
    {
      "assetId": "1984",
      "balance": "25000000",
      "isFrozen": false,
      "isSufficient": true
    }
  ]
}
```

## Response Fields

| Field                  | Type    | Description                                   |
| ---------------------- | ------- | --------------------------------------------- |
| assets                 | array   | Per-asset balance entries                     |
| assets\[].assetId      | string  | The asset's ID on Asset Hub                   |
| assets\[].balance      | string  | The account's balance of the asset            |
| assets\[].isFrozen     | boolean | Whether the balance is frozen                 |
| assets\[].isSufficient | boolean | Whether the asset is sufficient for existence |

## Use Cases

* **Token Wallets**: Show an account's Asset Hub token balances
* **Stablecoin UX**: Read USDT (asset 1984) or USDC balances
* **Portfolio Tools**: Aggregate asset holdings
* **Payments**: Confirm an asset balance before a transfer
