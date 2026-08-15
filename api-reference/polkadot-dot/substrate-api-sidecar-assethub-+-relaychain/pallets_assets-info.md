# pallets\_assets info

This endpoint returns the details and metadata of an Asset Hub asset, including its owner, total supply, minimum balance, and its name, symbol, and decimals.

{% hint style="info" %}
This endpoint is specific to Asset Hub (assets, foreign assets, and NFTs) and is served on the default network.
{% endhint %}

## Endpoint

```http
GET /pallets/assets/{assetId}/asset-info
```

## Path Parameters

| Parameter | Type   | Description               |
| --------- | ------ | ------------------------- |
| assetId   | string | The asset ID on Asset Hub |

## Query Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| at        | string | No       | Block height or hash to query at |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/pallets/assets/1984/asset-info'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/pallets/assets/1984/asset-info');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/pallets/assets/1984/asset-info')
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
  "assetInfo": {
    "owner": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5",
    "issuer": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5",
    "admin": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5",
    "freezer": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5",
    "supply": "12000000000000",
    "minBalance": "100",
    "isSufficient": true,
    "accounts": "45000",
    "status": "Live"
  },
  "assetMetadata": {
    "deposit": "0",
    "name": "Tether USD",
    "symbol": "USDt",
    "decimals": "6",
    "isFrozen": false
  }
}
```

## Response Fields

| Field                  | Type   | Description                       |
| ---------------------- | ------ | --------------------------------- |
| assetInfo.owner        | string | The asset's owner account         |
| assetInfo.supply       | string | Total supply of the asset         |
| assetInfo.minBalance   | string | Minimum balance to hold the asset |
| assetMetadata.name     | string | Human-readable asset name         |
| assetMetadata.symbol   | string | Asset ticker symbol               |
| assetMetadata.decimals | string | Decimal places of the asset       |

## Use Cases

* **Token Metadata**: Read an asset's name, symbol, and decimals
* **Stablecoin Display**: Show USDT or USDC supply and metadata
* **Wallets**: Resolve asset details for balance display
* **Analytics**: Track asset supply and holder counts
