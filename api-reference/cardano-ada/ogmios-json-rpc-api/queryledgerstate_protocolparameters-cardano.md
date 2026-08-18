---
description: >-
  Example code for the queryLedgerState_protocolParameters JSON-RPC method.
  Complete guide on how to use queryLedgerState_protocolParameters JSON-RPC in
  GetBlock Web3 documentation.
---

# queryLedgerState\_protocolParameters - Cardano

This method returns the current protocol parameters, including fee coefficients, execution unit prices, and size limits used to validate and price transactions.

## Parameters

This method does not require parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/protocolParameters",
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/protocolParameters", "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryLedgerState/protocolParameters", "id": "getblock.io"}
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
    "method": "queryLedgerState/protocolParameters",
    "result": {
        "minFeeCoefficient": 44,
        "minFeeConstant": {
            "ada": {
                "lovelace": 155381
            }
        },
        "maxBlockBodySize": {
            "bytes": 90112
        },
        "maxTransactionSize": {
            "bytes": 16384
        },
        "stakeCredentialDeposit": {
            "ada": {
                "lovelace": 2000000
            }
        },
        "stakePoolDeposit": {
            "ada": {
                "lovelace": 500000000
            }
        },
        "minUtxoDepositCoefficient": 4310,
        "scriptExecutionPrices": {
            "memory": "577/10000",
            "cpu": "721/10000000"
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field                 | Type    | Description                               |
| --------------------- | ------- | ----------------------------------------- |
| minFeeCoefficient     | integer | Fee charged per byte of transaction size  |
| minFeeConstant        | object  | Flat fee component in lovelace            |
| maxTransactionSize    | object  | Maximum transaction size in bytes         |
| stakePoolDeposit      | object  | Deposit required to register a stake pool |
| scriptExecutionPrices | object  | Prices for script memory and CPU units    |

## Use Cases

* **Fee Calculation**: Compute the exact fee for a transaction before building it
* **Size Validation**: Reject transactions that exceed size limits client-side
* **Script Budgeting**: Price Plutus script execution using unit prices
* **Deposit Handling**: Read stake and pool deposits when building certificates

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
