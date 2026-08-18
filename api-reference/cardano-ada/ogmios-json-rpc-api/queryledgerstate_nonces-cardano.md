---
description: >-
  Example code for the queryLedgerState_nonces JSON-RPC method. Complete guide
  on how to use queryLedgerState_nonces JSON-RPC in GetBlock Web3 documentation.
---

# queryLedgerState\_nonces - Cardano

This method returns the epoch nonce values used by the consensus protocol, which seed leader election.

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
    "method": "queryLedgerState/nonces",
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/nonces", "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryLedgerState/nonces", "id": "getblock.io"}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/nonces",
    "result": {
        "epochNonce": "bb5d68f96f32e55a140e9de9520ee98ba6a630b87580a6c5917e51eee436da55",
        "candidateNonce": "57a71679914e97740054b821f438f955ebb9a1bd4f530cbd83e35072deb17179",
        "evolvingNonce": "57a71679914e97740054b821f438f955ebb9a1bd4f530cbd83e35072deb17179",
        "lastEpochLastAncestor": "d4d0f950488bfbec0e859c27de458ee54160d58e21c3627339854500903d38a4"
    },
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field          | Type    | Description                               |
| -------------- | ------- | ----------------------------------------- |
| epoch          | integer | Epoch the nonces apply to                 |
| candidateNonce | string  | The candidate nonce for the next epoch    |
| evolvingNonce  | string  | The nonce evolving over the current epoch |

## Use Cases

* **Consensus Research**: Study leader-election seeding
* **Verification**: Independently verify slot leader schedules
* **Analytics**: Track nonce evolution across epochs
* **Auditing**: Confirm nonce values against block headers

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
