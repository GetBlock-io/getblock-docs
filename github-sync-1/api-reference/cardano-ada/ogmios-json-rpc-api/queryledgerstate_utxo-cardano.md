---
description: >-
  Example code for the queryLedgerState_utxo JSON-RPC method. Complete guide on
  how to use queryLedgerState_utxo JSON-RPC in GetBlock Web3 documentation.
---

# queryLedgerState\_utxo - Cardano

This method returns unspent transaction outputs, filtered by address or by output reference. It is the primary method for finding spendable inputs for a transaction.

## Parameters

| Parameter        | Type  | Required | Description                                            |
| ---------------- | ----- | -------- | ------------------------------------------------------ |
| addresses        | array | No       | Filter outputs to these addresses                      |
| outputReferences | array | No       | Filter outputs to these transaction id and index pairs |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/utxo",
    "params": {
        "addresses": [
            "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"
        ]
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/utxo", "params": {"addresses": ["addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"]}, "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryLedgerState/utxo", "params": {"addresses": ["addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q"]}, "id": "getblock.io"}
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
    "method": "queryLedgerState/utxo",
    "result": [
        {
            "transaction": {
                "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f"
            },
            "index": 0,
            "address": "addr1qxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4j5k6l7m8n9p0q",
            "value": {
                "ada": {
                    "lovelace": 9407625
                }
            }
        }
    ],
    "id": "getblock.io"
}
```

## Response Parameters

| Field       | Type    | Description                                       |
| ----------- | ------- | ------------------------------------------------- |
| transaction | object  | The transaction that created the output           |
| index       | integer | Output index within that transaction              |
| address     | string  | Address that owns the output                      |
| value       | object  | Output value, including ada and any native assets |

## Use Cases

* **Coin Selection**: Read spendable UTXOs when building a transaction
* **Balance Construction**: Sum output values to compute an address balance
* **Asset Discovery**: Find native assets held at an address
* **Wallet Backends**: Source inputs for transaction construction

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
