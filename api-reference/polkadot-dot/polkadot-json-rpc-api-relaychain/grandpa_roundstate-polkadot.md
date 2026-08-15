---
description: >-
  Example code for the grandpa_roundState JSON RPC method. Complete guide on how
  to use grandpa_roundState JSON RPC in GetBlock Web3 documentation.
---

# grandpa\_roundState - Polkadot

This method returns the state of the current GRANDPA voting round, including prevotes and precommits and their weights. GRANDPA is Polkadot's finality gadget.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "grandpa_roundState", "params": [], "id": "getblock.io"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        jsonrpc: '2.0',
        method: 'grandpa_roundState',
        params: [],
        id: 'getblock.io'
    })
});

const data = await response.json();
console.log(data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'grandpa_roundState',
        'params': [],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "setId": 12034,
        "best": {
            "round": 4821,
            "totalWeight": 297,
            "thresholdWeight": 199,
            "prevotes": {
                "currentWeight": 240,
                "missing": []
            },
            "precommits": {
                "currentWeight": 210,
                "missing": []
            }
        },
        "background": []
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| id        | string | Request identifier matching the request |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| result    | object | GRANDPA round state object              |

### best

| Field           | Type    | Description                         |
| --------------- | ------- | ----------------------------------- |
| round           | integer | Current GRANDPA round number        |
| totalWeight     | integer | Total voter weight in the round     |
| thresholdWeight | integer | Weight required to finalize         |
| prevotes        | object  | Prevote weight and missing voters   |
| precommits      | object  | Precommit weight and missing voters |

## Use Cases

* **Finality Monitoring**: Track progress toward finalizing a block
* **Validator Analytics**: Measure voter participation in a round
* **Network Diagnostics**: Investigate stalled finality
* **Dashboards**: Show GRANDPA round progress

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.grandpa.roundState`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.grandpa.roundState();
console.log(result.toHuman());
```
{% endcode %}
