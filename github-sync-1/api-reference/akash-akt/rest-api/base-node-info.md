---
description: >-
  Example code for the cosmos/base/tendermint/v1beta1/node_info REST method.
  Complete guide on how to use cosmos/base/tendermint/v1beta1/node_info REST
  method in GetBlock Web3 documentation.
---

# /cosmos/base/tendermint/v1beta1/node\_info - Akash

Returns node and application version info via the Cosmos base service.

## Endpoint

```http
GET /cosmos/base/tendermint/v1beta1/node_info
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/base/tendermint/v1beta1/node_info"
```
{% endcode %}

## Response

```json
{
    "default_node_info": {
        "protocol_version": {
            "p2p": "8",
            "block": "11",
            "app": "0"
        },
        "default_node_id": "821fa0f7ce74a211c5f5ec93cc6cc301564b92b6",
        "listen_addr": "0.0.0.0:26656",
        "network": "akashnet-2",
        "version": "0.38.19",
        "channels": "QCAhIiMwOGBhAA==",
        "moniker": "Tendermint",
        "other": {
            "tx_index": "on",
            "rpc_address": "tcp://0.0.0.0:26657"
        }
    },
    "application_version": {
        "name": "akash",
        "app_name": "akash",
        "version": "2.1.0",
        "git_commit": "151b989a58f579ac08b688b325e0dfcc4b44fecb",
        "build_tags": "osusergo,netgo,ledger,muslc,gcc",
        "go_version": "go version go1.26.4 linux/amd64",
        "build_deps": [
            {
                "path": "cel.dev/expr",
                "version": "v0.24.0",
                "sum": "h1:56OvJKSH3hDGL0ml5uSxZmz3/3Pq4tJ+fb1unVLAFcY="
            }
        ],
        "cosmos_sdk_version": "v0.53.7-akash.2"
    }
}
```

## Response Fields

| Field                | Type   | Description              |
| -------------------- | ------ | ------------------------ |
| default\_node\_info  | object | Node network and version |
| application\_version | object | Application build info   |

## Use Cases

* **Diagnostics**: Report node/app versions

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
