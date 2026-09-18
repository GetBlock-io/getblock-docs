---
description: >-
  Example code for the getInfo WebSocket method. Complete guide on how to use
  the getInfo WebSocket method in the GetBlock Web3 documentation.
---

# getInfo - Dogecoin

Returns the indexer's identity and sync state, together with the Dogecoin Core node version. Use it as a connection health check and to confirm the indexer has caught up before trusting an address query.

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
        "name": "Dogecoin",
        "shortcut": "DOGE",
        "network": "DOGE",
        "decimals": 8,
        "version": "unknown",
        "bestHeight": 6378961,
        "bestHash": "7603e59cec9c7a411b052acc187b27def866ef5d4653711a619c57801a65dfbf",
        "block0Hash": "1a91e3dace36e2be3bf030a65679fe821aa1d6ef92e7c9902eb318182c355691",
        "testnet": false,
        "backend": {
            "version": "1140900",
            "subversion": "/Shibetoshi:1.14.9/"
        }
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| name | string | Coin name |
| shortcut | string | Coin ticker |
| network | string | Network identifier |
| decimals | integer | Decimal places in the coin's base unit |
| bestHeight | integer | Height of the best block the indexer has processed |
| bestHash | string | Hash of the best block the indexer has processed |
| block0Hash | string | Genesis block hash, used to confirm the expected chain |
| testnet | boolean | True when the endpoint serves a test network |
| backend | object | Dogecoin Core version and subversion |

{% hint style="info" %}
The node identifies itself as `/Shibetoshi:1.14.9/` — that is Dogecoin Core's user agent, not a different implementation. `version` reports `unknown` on the shared deployment, so use `backend.subversion` to identify the node.
{% endhint %}

## Use Cases

* **Health Checks**: Confirm the connection and the indexer are live
* **Sync Verification**: Compare `bestHeight` against an independent source
* **Network Assertion**: Check `block0Hash` and `testnet` to confirm the chain
* **Diagnostics**: Report indexer and node versions in support requests

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
