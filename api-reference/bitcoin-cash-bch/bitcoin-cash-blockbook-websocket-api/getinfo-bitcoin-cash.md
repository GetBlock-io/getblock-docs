---
description: >-
  Example code for the getInfo WebSocket method. Complete guide on how to use
  the getInfo WebSocket method in the GetBlock Web3 documentation.
---

# getInfo - Bitcoin Cash

Returns the indexer's identity and sync state, together with the backend node version. Use it as a connection health check and to confirm the indexer has caught up to the chain tip before trusting an address query.

## Parameters

This method takes no parameters. Send an empty `params` object.

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getInfo",
    "params": {}
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "name": "Bcash",
        "shortcut": "BCH",
        "decimals": 8,
        "version": "unknown",
        "bestHeight": 968994,
        "bestHash": "00000000000000000143166818d12134d3863648adeb2903e8191e01d9ca0602",
        "block0Hash": "000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f",
        "testnet": false,
        "backend": {
            "version": "29010000",
            "subversion": "/Bitcoin Cash Node:29.1.0(EB32.0)/"
        }
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| name | string | Coin name as the indexer reports it |
| shortcut | string | Coin ticker |
| decimals | integer | Decimal places in the coin's base unit |
| bestHeight | integer | Height of the best block the indexer has processed |
| bestHash | string | Hash of the best block the indexer has processed |
| block0Hash | string | Genesis block hash |
| testnet | boolean | True when the endpoint serves a test network |
| backend | object | Backend node version and subversion |

{% hint style="info" %}
Two quirks are worth knowing. `name` is reported as `Bcash`, the indexer's internal coin name, not "Bitcoin Cash". And unlike the other Blockbook chains this response carries **no `network` field**, so do not key logic on its presence; use `shortcut` and `block0Hash` to identify the chain instead.

`block0Hash` is the Bitcoin genesis block, because Bitcoin Cash shares history with Bitcoin up to the 2017 fork. It therefore cannot distinguish BCH from BTC on its own.
{% endhint %}

## Use Cases

* **Health Checks**: Confirm the connection and the indexer are live
* **Sync Verification**: Compare `bestHeight` against an independent source before trusting a balance
* **Diagnostics**: Report indexer and node versions in support requests

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
