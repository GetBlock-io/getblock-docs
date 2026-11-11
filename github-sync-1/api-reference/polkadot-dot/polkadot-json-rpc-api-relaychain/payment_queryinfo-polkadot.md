---
description: >-
  Example code for the payment_queryInfo JSON RPC method. Complete guide on how
  to use payment_queryInfo JSON RPC in GetBlock Web3 documentation.
---

# payment\_queryInfo - Polkadot

This method returns the weight and estimated fee for a submitted extrinsic. It is used to estimate the cost of a transaction before signing.

## Parameters

| Parameter | Type   | Required | Description                                                       |
| --------- | ------ | -------- | ----------------------------------------------------------------- |
| extrinsic | string | Yes      | A SCALE-encoded extrinsic, hex-encoded                            |
| at        | string | No       | Block hash to query at. Defaults to the latest block when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "payment_queryInfo", "params": ["0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."], "id": "getblock.io"}'
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
        method: 'payment_queryInfo',
        params: ["0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."],
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
        'method': 'payment_queryInfo',
        'params': ["0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."],
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
        "weight": {
            "refTime": 133700000,
            "proofSize": 3593
        },
        "class": "Normal",
        "partialFee": "15960000000"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                       |
| --------- | ------ | ------------------------------------------------- |
| id        | string | Request identifier matching the request           |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                 |
| result    | object | Runtime dispatch info with weight and partial fee |

### Result Object

| Field      | Type   | Description                                     |
| ---------- | ------ | ----------------------------------------------- |
| weight     | object | Dispatch weight (refTime and proofSize)         |
| class      | string | Dispatch class (Normal, Operational, Mandatory) |
| partialFee | string | Estimated fee in Planck, excluding tip          |

## Use Cases

* **Fee Estimation**: Estimate a transaction's fee before signing
* **Cost Preview**: Show users the expected fee
* **Budgeting**: Compute fees for batches of transactions
* **Weight Analysis**: Read the dispatch weight of an extrinsic

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.payment.queryInfo`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.payment.queryInfo('0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01...');
console.log(result.toHuman());
```
{% endcode %}
