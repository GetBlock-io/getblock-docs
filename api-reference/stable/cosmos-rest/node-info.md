---
description: >-
  Example code for the node info REST method. Complete guide on how to use node
  info REST method in GetBlock Web3 documentation.
---

# node info

Returns the node's information and the application's version details via the Cosmos base tendermint service.

## Endpoint

```http
GET /cosmos/base/tendermint/v1beta1/node_info
```

## Example

{% code overflow="wrap" %}
```bash
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/base/tendermint/v1beta1/node_info"
```
{% endcode %}

## Response

```json
{
    "default_node_info": {
        "network": "stablechain_988-1",
        "moniker": "cosmos-node",
        "version": "0.38.19"
    },
    "application_version": {
        "name": "stabled",
        "version": "29.0.0",
        "cosmos_sdk_version": "v0.50.x"
    }
}
```

## Response Fields

| Field                | Type   | Description                                                   |
| -------------------- | ------ | ------------------------------------------------------------- |
| default\_node\_info  | object | Node network (chain id), moniker, and CometBFT version        |
| application\_version | object | The stabled application name, version, and Cosmos SDK version |

## Use Cases

* **Version discovery**: Read the stabled and SDK versions the node runs
* **Chain identity**: Confirm the network is stablechain\_988-1

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
