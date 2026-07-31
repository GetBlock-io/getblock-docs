---
description: >-
  Example code for the network/status JSON-RPC method. Complete guide on how to
  use network/status JSON-RPC in GetBlock Web3 documentation.
---

# network/status - Cardano

This endpoint returns the current status of the network, including the current and genesis block identifiers, the latest block timestamp, and connected peers.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type   | Required | Description                                       |
| ------------------- | ------ | -------- | ------------------------------------------------- |
| network\_identifier | object | Yes      | The network to query, with blockchain and network |
| metadata            | object | No       | Optional metadata object                          |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/network/status' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "metadata": {}
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/network/status',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "metadata": {}})
    }
);
console.log(await response.json());
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/network/status',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "metadata": {}}
)

print(response.json())
```
{% endtab %}
{% endtabs %}

## Response

```json
{
    "current_block_identifier": {
        "index": 10453789,
        "hash": "6e9e89632bc5c72030d3a486647e889c48d63e4da0643191b13566ad816d2d57"
    },
    "current_block_timestamp": 1617180599000,
    "genesis_block_identifier": {
        "index": 0,
        "hash": "5f20df933584822601f9e3f8c024eb5eb252fe8cefb24d1317dc3d432e940ebb"
    },
    "peers": [
        {
            "peer_id": "relay.example:3001"
        }
    ]
}
```

## Response Parameters

| Field                      | Type    | Description                                |
| -------------------------- | ------- | ------------------------------------------ |
| current\_block\_identifier | object  | Index and hash of the current tip block    |
| current\_block\_timestamp  | integer | Timestamp of the tip block in milliseconds |
| genesis\_block\_identifier | object  | Index and hash of the genesis block        |
| peers                      | array   | Peers the node is connected to             |

## Use Cases

* **Tip Tracking**: Read the current block index and hash
* **Sync Checks**: Compare the tip against a local index
* **Timestamps**: Read the tip block time for display
* **Health Monitoring**: Inspect connected peers and chain progress

## Error Handling

| Error Code | Message         | Description                                              |
| ---------- | --------------- | -------------------------------------------------------- |
| 1          | Invalid request | The request body is malformed or missing required fields |
| 4          | Block not found | The requested block does not exist on this node          |
| 5          | Internal error  | The node failed to answer the request                    |
