---
description: >-
  Example code for the network_info JSON-RPC method. Complete guide on how to
  use network_info JSON-RPC in GetBlock Web3 documentation.
---

# network\_info - NEAR Protocol

Returns the node's current network connections: active peers, known producers, and sent/received data counts.

## Method

```
network_info
```

## Parameters

This method takes an empty params array `[]`.

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "network_info", "params": []}'
```
{% endcode %}

## Response

```json
{
    "active_peers": [
        {
            "id": "ed25519:...",
            "addr": "1.2.3.4:24567"
        }
    ],
    "num_active_peers": 24,
    "peer_max_count": 40,
    "sent_bytes_per_sec": 12345,
    "received_bytes_per_sec": 23456,
    "known_producers": []
}
```

## Response Fields

| Field              | Type   | Description               |
| ------------------ | ------ | ------------------------- |
| active\_peers      | array  | Currently connected peers |
| num\_active\_peers | number | Number of active peers    |
| peer\_max\_count   | number | Maximum peer connections  |
| known\_producers   | array  | Known block producers     |

## Use Cases

* **Connectivity**: Confirm the node has healthy peering
* **Diagnostics**: Inspect peer counts and throughput

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
