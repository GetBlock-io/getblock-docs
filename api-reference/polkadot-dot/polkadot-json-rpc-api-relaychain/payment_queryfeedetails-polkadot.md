---
description: >-
  Example code for the payment_queryFeeDetails JSON RPC method. Complete guide
  on how to use payment_queryFeeDetails JSON RPC in GetBlock Web3 documentation.
---

# payment\_queryFeeDetails - Polkadot

This method returns the breakdown of the inclusion fee for an extrinsic: the base fee, length fee, and adjusted weight fee.

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
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "payment_queryFeeDetails", "params": ["0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."], "id": "getblock.io"}'
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
        method: 'payment_queryFeeDetails',
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'payment_queryFeeDetails',
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
        "inclusionFee": {
            "baseFee": "1000000000",
            "lenFee": "960000000",
            "adjustedWeightFee": "14000000000"
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                        |
| --------- | ------ | -------------------------------------------------- |
| id        | string | Request identifier matching the request            |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                  |
| result    | object | Fee detail object with the inclusion-fee breakdown |

### inclusionFee

| Field             | Type   | Description                                  |
| ----------------- | ------ | -------------------------------------------- |
| baseFee           | string | Fixed base fee in Planck                     |
| lenFee            | string | Fee for the encoded length, in Planck        |
| adjustedWeightFee | string | Weight-based fee after adjustment, in Planck |

## Use Cases

* **Fee Breakdown**: Show users how a fee is composed
* **Cost Modeling**: Model fees across transaction sizes
* **Accounting**: Attribute fee components for reporting
* **Optimization**: Reduce length or weight to lower fees

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.payment.queryFeeDetails`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.payment.queryFeeDetails('0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01...');
console.log(result.toHuman());
```
{% endcode %}
