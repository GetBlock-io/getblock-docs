---
description: >-
  Example code for the getaccountresource REST method. Complete guide on how to
  use getaccountresource REST method in GetBlock Web3 documentation.
---

# /wallet/getaccountresource - Tron

This endpoint returns an account's resource state: its Energy and Bandwidth limits, current usage, and the total network resource pools. Energy powers smart-contract execution and Bandwidth covers transaction size.

{% hint style="info" %}
This is a read endpoint. It is also served by the Solidity node at `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/walletsolidity/getaccountresource`, which returns only confirmed, irreversible data. Use the Solidity node for balance and payment verification.
{% endhint %}

## Parameters

| Parameter | Type    | Required | Description                                                 |
| --------- | ------- | -------- | ----------------------------------------------------------- |
| address   | string  | Yes      | Account address, base58 when visible is true                |
| visible   | boolean | No       | Return and accept addresses in base58 format. Default false |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getaccountresource' \
--header 'Content-Type: application/json' \
--data-raw '{
  "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "visible": true
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getaccountresource',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "visible": true})
    }
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getaccountresource',
    headers={'Content-Type': 'application/json'},
    json={"address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "visible": true}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "freeNetLimit": 600,
  "NetLimit": 1500,
  "NetUsed": 200,
  "EnergyLimit": 50000,
  "EnergyUsed": 12000,
  "TotalNetLimit": 43200000000,
  "TotalEnergyLimit": 180000000000,
  "tronPowerLimit": 100
}
```

## Response Parameters

| Field            | Type    | Description                               |
| ---------------- | ------- | ----------------------------------------- |
| freeNetLimit     | integer | Free Bandwidth available to every account |
| NetLimit         | integer | Bandwidth available from staked TRX       |
| EnergyLimit      | integer | Energy available from staked TRX          |
| EnergyUsed       | integer | Energy consumed in the current window     |
| TotalEnergyLimit | integer | Total Energy pool across the network      |

## Use Cases

* **Fee Preflight**: Check available Energy before a contract call
* **Resource Planning**: Decide how much TRX to stake for Energy or Bandwidth
* **Transaction Sizing**: Confirm Bandwidth covers a transaction's size
* **Monitoring**: Track resource consumption for an account

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
