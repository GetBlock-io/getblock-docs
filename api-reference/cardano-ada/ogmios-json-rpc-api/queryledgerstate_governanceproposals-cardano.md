---
description: >-
  Example code for the queryLedgerState_governanceProposals JSON-RPC method.
  Complete guide on how to use queryLedgerState_governanceProposals JSON-RPC in
  GetBlock Web3 documentation.
---

# queryLedgerState\_governanceProposals - Cardano

This method returns the currently active governance proposals under the Conway-era on-chain governance system.

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
    "method": "queryLedgerState/governanceProposals",
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/governanceProposals", "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryLedgerState/governanceProposals", "id": "getblock.io"}
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
    "method": "queryLedgerState/governanceProposals",
    "result": [
        {
            "proposal": {
                "transaction": {
                    "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f"
                },
                "index": 0
            },
            "deposit": {
                "ada": {
                    "lovelace": 100000000000
                }
            },
            "returnAccount": "stake1uxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4",
            "action": {
                "type": "treasuryWithdrawals"
            }
        }
    ],
    "id": "getblock.io"
}
```

## Response Parameters

| Field         | Type   | Description                                      |
| ------------- | ------ | ------------------------------------------------ |
| proposal      | object | The governance action's transaction id and index |
| deposit       | object | Deposit locked for the proposal                  |
| returnAccount | string | Account the deposit returns to                   |
| action        | object | The governance action type and its parameters    |

## Use Cases

* **Governance Tools**: List active proposals for voters
* **DRep Interfaces**: Show actions a delegate representative can vote on
* **Monitoring**: Track proposals through their lifecycle
* **Analytics**: Report on governance activity

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
