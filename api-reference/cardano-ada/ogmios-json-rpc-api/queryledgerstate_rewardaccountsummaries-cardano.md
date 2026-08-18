---
description: >-
  Example code for the queryLedgerState_rewardAccountSummaries JSON-RPC method.
  Complete guide on how to use queryLedgerState_rewardAccountSummaries JSON-RPC
  in GetBlock Web3 documentation.
---

# queryLedgerState\_rewardAccountSummaries - Cardano

This method returns reward-account summaries for the given stake credentials, including the withdrawable rewards balance and current delegation.

## Parameters

| Parameter | Type  | Required | Description                               |
| --------- | ----- | -------- | ----------------------------------------- |
| keys      | array | No       | Stake credentials by bech32 stake address |
| scripts   | array | No       | Stake credentials by script hash          |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/rewardAccountSummaries",
    "params": {
        "keys": [
            "stake1uxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4"
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/rewardAccountSummaries", "params": {"keys": ["stake1uxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4"]}, "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryLedgerState/rewardAccountSummaries", "params": {"keys": ["stake1uxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4"]}, "id": "getblock.io"}
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
    "method": "queryLedgerState/rewardAccountSummaries",
    "result": {
        "stake1uxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4": {
            "delegate": {
                "id": "pool1abc..."
            },
            "rewards": {
                "ada": {
                    "lovelace": 4521000
                }
            },
            "deposit": {
                "ada": {
                    "lovelace": 2000000
                }
            }
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field    | Type   | Description                                        |
| -------- | ------ | -------------------------------------------------- |
| delegate | object | The stake pool the account currently delegates to  |
| rewards  | object | Withdrawable rewards balance in lovelace           |
| deposit  | object | Registration deposit held for the stake credential |

## Use Cases

* **Rewards Display**: Show a delegator's withdrawable rewards
* **Withdrawal Flows**: Read the rewards balance before building a withdrawal
* **Delegation Status**: Confirm which pool an account delegates to
* **Wallet Backends**: Track staking state for an account

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
