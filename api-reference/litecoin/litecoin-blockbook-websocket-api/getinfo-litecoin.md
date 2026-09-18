---
description: >-
  Example code for the getInfo WebSocket method. Complete guide on how to use
  the getInfo WebSocket method in the GetBlock Web3 documentation.
---

# getInfo - Litecoin

Returns the indexer's identity and sync state, together with the Litecoin Core node version. Use it as a connection health check and to confirm the indexer has caught up before trusting an address query.

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
        "name": "Litecoin",
        "shortcut": "LTC",
        "network": "LTC",
        "decimals": 8,
        "version": "unknown",
        "bestHeight": 3179816,
        "bestHash": "3e469b185937924138e5ab9b4fda4901dc2ad5507c7190fb56968c0654a0a1ac",
        "block0Hash": "12a765e31ffd4059bada1e25190f6e98c99d9714d334efa41a195a7e7e04bfe2",
        "testnet": false,
        "backend": {
            "version": "210508",
            "subversion": "/LitecoinCore:0.21.5.8/"
        }
    }
}
```

## Response Fields

| Field      | Type    | Description                                            |
| ---------- | ------- | ------------------------------------------------------ |
| name       | string  | Coin name                                              |
| shortcut   | string  | Coin ticker                                            |
| decimals   | integer | Decimal places in the coin's base unit                 |
| bestHeight | integer | Height of the best block the indexer has processed     |
| bestHash   | string  | Hash of the best block the indexer has processed       |
| block0Hash | string  | Genesis block hash, used to confirm the expected chain |
| testnet    | boolean | True when the endpoint serves a test network           |
| backend    | object  | Litecoin Core version and subversion                   |

{% hint style="info" %}
`version` reports `unknown` on the shared deployment. Identify the node through `backend.subversion` instead.
{% endhint %}

## Use Cases

* **Health Checks**: Confirm the connection and the indexer are live
* **Sync Verification**: Compare `bestHeight` against an independent source
* **Network Assertion**: Check `block0Hash` and `testnet` to confirm the chain
* **Diagnostics**: Report indexer and node versions in support requests

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
