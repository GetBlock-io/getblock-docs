---
description: >-
  Example code for the getInfo WebSocket method. Complete guide on how to use
  the getInfo WebSocket method in the GetBlock Web3 documentation.
---

# getInfo - Bitcoin

Returns the indexer's identity and sync state, together with the backend node version. Use it as a connection health check and to confirm the indexer has caught up to the chain tip before trusting an address query.

## Parameters

This method takes no parameters. Send an empty `params` object.

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

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
        "name": "Bitcoin",
        "shortcut": "BTC",
        "network": "BTC",
        "decimals": 8,
        "version": "unknown",
        "bestHeight": 967487,
        "bestHash": "000000000000000000017cfd38e8af73159da4d9ab4c3f82ae183659f7b09793",
        "block0Hash": "000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f",
        "testnet": false,
        "backend": {
            "version": "310100",
            "subversion": "/Satoshi:31.1.0/"
        }
    }
}
```

## Response Fields

| Field      | Type    | Description                                                 |
| ---------- | ------- | ------------------------------------------------------------- |
| name       | string  | Coin name                                                   |
| shortcut   | string  | Coin ticker                                                 |
| network    | string  | Network identifier                                          |
| decimals   | integer | Decimal places in the coin's base unit                      |
| version    | string  | Blockbook indexer version                                   |
| bestHeight | integer | Height of the best block the indexer has processed          |
| bestHash   | string  | Hash of the best block the indexer has processed            |
| block0Hash | string  | Genesis block hash, used to confirm the expected chain      |
| testnet    | boolean | True when the endpoint serves a test network                |
| backend    | object  | Backend node version and subversion                         |

## Use Cases

* **Health Checks**: Confirm the connection and the indexer are live
* **Sync Verification**: Compare `bestHeight` against an independent source before trusting a balance
* **Network Assertion**: Check `block0Hash` and `testnet` to confirm the endpoint serves the expected chain
* **Diagnostics**: Report indexer and backend versions in support requests

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
