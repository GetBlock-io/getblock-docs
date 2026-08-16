---
description: >-
  Example code for the account_nextIndex JSON RPC method. Complete guide on how
  to use account_nextIndex JSON RPC in GetBlock Web3 documentation.
---

# account\_nextIndex - Polkadot

This method returns the next nonce for an account, including any transactions already queued in the pool. It is an alias of `system_accountNextIndex`.

## Parameters

| Parameter | Type   | Required | Description          |
| --------- | ------ | -------- | -------------------- |
| accountId | string | Yes      | SS58 account address |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "account_nextIndex", "params": ["15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5"], "id": "getblock.io"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        jsonrpc: '2.0',
        method: 'account_nextIndex',
        params: ["15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5"],
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'account_nextIndex',
        'params': ["15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5"],
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
    "result": 1234
}
```

## Response Parameters

| Parameter | Type    | Description                                               |
| --------- | ------- | --------------------------------------------------------- |
| id        | string  | Request identifier matching the request                   |
| jsonrpc   | string  | JSON-RPC protocol version ("2.0")                         |
| result    | integer | The next nonce (transaction index) to use for the account |

## Use Cases

* **Transaction Building**: Set the nonce for a new extrinsic
* **Sequencing**: Order multiple extrinsics from one account
* **Pool Awareness**: Account for pending transactions in the nonce
* **Wallet Backends**: Compute nonces server-side before signing

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.system.accountNextIndex`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.system.accountNextIndex('15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5');
console.log(result.toHuman());
```
{% endcode %}
