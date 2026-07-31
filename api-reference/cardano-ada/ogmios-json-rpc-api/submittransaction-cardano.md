---
description: >-
  Example code for the submitTransaction JSON-RPC method. Complete guide on how
  to use submitTransaction JSON-RPC in GetBlock Web3 documentation.
---

# submitTransaction - Cardano

This method submits a signed, serialized transaction to the network. On success it returns the transaction id; on failure it returns the list of validation errors reported by the node.

## Parameters

| Parameter   | Type   | Required | Description                                             |
| ----------- | ------ | -------- | ------------------------------------------------------- |
| transaction | object | Yes      | Object with a cbor field holding the base16 transaction |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "submitTransaction",
    "params": {
        "transaction": {
            "cbor": "84a300818258200e...transaction-cbor-hex...ff"
        }
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"jsonrpc": "2.0", "method": "submitTransaction", "params": {"transaction": {"cbor": "84a300818258200e...transaction-cbor-hex...ff"}}, "id": "getblock.io"})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={"jsonrpc": "2.0", "method": "submitTransaction", "params": {"transaction": {"cbor": "84a300818258200e...transaction-cbor-hex...ff"}}, "id": "getblock.io"}
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
    "method": "submitTransaction",
    "result": {
        "transaction": {
            "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f"
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field       | Type   | Description                                   |
| ----------- | ------ | --------------------------------------------- |
| transaction | object | Object containing the accepted transaction id |

## Use Cases

* **Payment Broadcast**: Submit a signed ada or asset transfer
* **dApp Backends**: Broadcast transactions built and signed client-side
* **Certificate Submission**: Submit staking and governance certificates
* **Retry Flows**: Resubmit a dropped transaction from stored CBOR

## Error Handling

| Error Code | Message            | Description                                                               |
| ---------- | ------------------ | ------------------------------------------------------------------------- |
| -32602     | Invalid params     | The transaction CBOR is missing or malformed                              |
| 3005       | Era mismatch       | The transaction was built for a different ledger era                      |
| 3100       | Validation failure | The node rejected the transaction; see the failure list in the error data |
| -32603     | Internal error     | The node failed to process the submission                                 |
