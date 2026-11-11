---
description: >-
  Example code for the queryLedgerState_stakePools JSON-RPC method. Complete
  guide on how to use queryLedgerState_stakePools JSON-RPC in GetBlock Web3
  documentation.
---

# queryLedgerState\_stakePools - Cardano

This method returns the registered stake pools and their current parameters, such as pledge, cost, and margin.

## Parameters

| Parameter  | Type  | Required | Description                                           |
| ---------- | ----- | -------- | ----------------------------------------------------- |
| stakePools | array | No       | Filter to specific stake pool ids. Omit for all pools |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/stakePools",
    "params": {
        "stakePools": []
    },
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/stakePools", "params": {"stakePools": []}, "id": "getblock.io"})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={"jsonrpc": "2.0", "method": "queryLedgerState/stakePools", "params": {"stakePools": []}, "id": "getblock.io"}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/stakePools",
    "result": {
        "pool1abc...": {
            "id": "pool1abc...",
            "cost": {
                "ada": {
                    "lovelace": 340000000
                }
            },
            "margin": "3/100",
            "pledge": {
                "ada": {
                    "lovelace": 100000000000
                }
            }
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field  | Type   | Description                                |
| ------ | ------ | ------------------------------------------ |
| id     | string | Bech32 stake pool identifier               |
| cost   | object | Fixed operating cost per epoch in lovelace |
| margin | string | Operator margin as a ratio                 |
| pledge | object | Amount the operator has pledged            |

## Use Cases

* **Delegation Tools**: List pools and their terms for delegators
* **Pool Explorers**: Display pool parameters and pledge
* **Reward Estimation**: Feed pool cost and margin into reward models
* **Monitoring**: Track parameter changes across pools

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
