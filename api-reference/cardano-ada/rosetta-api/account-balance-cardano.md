---
description: >-
  Example code for the account/balance  JSON-RPC method. Complete guide on how
  to use account/balance  JSON-RPC in GetBlock Web3 documentation.
---

# account/balance - Cardano

This endpoint returns the balance of an account, optionally at a historical block. Balances include ada and any native assets held by the address.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type   | Required | Description                                        |
| ------------------- | ------ | -------- | -------------------------------------------------- |
| network\_identifier | object | Yes      | The network to query                               |
| account\_identifier | object | Yes      | The account, with an address field                 |
| block\_identifier   | object | No       | Partial identifier for a historical balance lookup |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/account/balance' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "account_identifier": {
        "address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"
    }
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/account/balance',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "account_identifier": {"address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"}})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/account/balance',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "account_identifier": {"address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"}}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "block_identifier": {
        "index": 10453789,
        "hash": "6e9e89632bc5c72030d3a486647e889c48d63e4da0643191b13566ad816d2d57"
    },
    "balances": [
        {
            "value": "71103107",
            "currency": {
                "symbol": "ADA",
                "decimals": 6
            }
        },
        {
            "value": "10000",
            "currency": {
                "symbol": "nutcoin",
                "decimals": 0,
                "metadata": {
                    "policyId": "b0d07d45fe9514f80213f4020e5a61241458be626841cde717cb38a7"
                }
            }
        }
    ]
}
```

## Response Parameters

| Field                | Type   | Description                                           |
| -------------------- | ------ | ----------------------------------------------------- |
| block\_identifier    | object | The block the balance was read at                     |
| balances             | array  | Balances by currency, including ada and native assets |
| balances\[].value    | string | Balance amount in the currency's base unit            |
| balances\[].currency | object | Currency symbol, decimals, and native-asset metadata  |

## Use Cases

* **Balance Display**: Show an address's ada and token balances
* **Historical Balances**: Read a balance at a past block
* **Exchange Accounting**: Track deposit address balances
* **Portfolio Tools**: Aggregate native-asset holdings

## Error Handling

| Error Code | Message                | Description                                                |
| ---------- | ---------------------- | ---------------------------------------------------------- |
| 12         | Invalid account format | The requested account identifier is improperly formatted   |
| 4          | Block not found        | The requested block does not exist for a historical lookup |
| 5          | Internal error         | The node failed to read account data                       |
