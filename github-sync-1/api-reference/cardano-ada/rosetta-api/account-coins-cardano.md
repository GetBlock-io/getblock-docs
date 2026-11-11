---
description: >-
  Example code for the account/coins JSON_RPC method. Complete guide on how to
  use account/coins JSON_RPC in GetBlock Web3 documentation.
---

# account/coins - Cardano

This endpoint returns the unspent outputs, or coins, owned by an address, including any native tokens attached to each output. Coins are the inputs available for building a transaction.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type    | Required | Description                                                     |
| ------------------- | ------- | -------- | --------------------------------------------------------------- |
| network\_identifier | object  | Yes      | The network to query                                            |
| account\_identifier | object  | Yes      | The account, with an address field                              |
| include\_mempool    | boolean | No       | Whether to reflect mempool spends. Breaks idempotency when true |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/account/coins' \
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
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/account/coins',
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/account/coins',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "account_identifier": {"address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"}}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code overflow="wrap" %}
```json
{
    "block_identifier": {
        "index": 10453789,
        "hash": "6e9e89632bc5c72030d3a486647e889c48d63e4da0643191b13566ad816d2d57"
    },
    "coins": [
        {
            "coin_identifier": {
                "identifier": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642:0"
            },
            "amount": {
                "value": "9407625",
                "currency": {
                    "symbol": "ADA",
                    "decimals": 6
                }
            }
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field                     | Type   | Description                          |
| ------------------------- | ------ | ------------------------------------ |
| block\_identifier         | object | The block the coins were read at     |
| coins                     | array  | Unspent outputs owned by the address |
| coins\[].coin\_identifier | object | The output reference as txhash:index |
| coins\[].amount           | object | The output value and currency        |

## Use Cases

* **Coin Selection**: Read spendable outputs when building a transaction
* **Balance Construction**: Sum coin values to compute a balance
* **Asset Discovery**: Find native tokens attached to outputs
* **Wallet Backends**: Source inputs for Rosetta construction

## Error Handling

| Error Code | Message                | Description                                                |
| ---------- | ---------------------- | ---------------------------------------------------------- |
| 12         | Invalid account format | The requested account identifier is improperly formatted   |
| 4          | Block not found        | The requested block does not exist for a historical lookup |
| 5          | Internal error         | The node failed to read account data                       |
