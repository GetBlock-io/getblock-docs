---
description: >-
  Example code for the getInfo WebSocket method. Complete guide on how to use
  the getInfo WebSocket method in the GetBlock Web3 documentation.
---

# getInfo - Zcash

Returns the indexer's identity and sync state, together with the backend node's consensus branch IDs. Use it as a connection health check and to confirm the indexer has caught up before trusting an address query.

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
        "name": "Zcash",
        "shortcut": "ZEC",
        "network": "ZEC",
        "decimals": 8,
        "version": "unknown",
        "bestHeight": 3489836,
        "bestHash": "000000000061b35bd0656948f37e2f8510905be8fc678c028481fa9e8f411f70",
        "block0Hash": "00040fe8ec8471911baa1db1266ea15dd06b4a8a5c453883c000b031973dce08",
        "testnet": false,
        "backend": {
            "version": "zebra",
            "consensus": {
                "chaintip": "37a5165b",
                "nextblock": "37a5165b"
            }
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
| block0Hash | string | Genesis block hash |
| testnet | boolean | True when the endpoint serves a test network |
| backend.version | string | Backend implementation. Reported as `zebra`, with no version number |
| backend.consensus.chaintip | string | Consensus branch ID in force at the chain tip |
| backend.consensus.nextblock | string | Consensus branch ID that will apply to the next block |

{% hint style="info" %}
Zcash is the only Blockbook chain whose `backend` carries a `consensus` object. Those values are consensus branch IDs, which change at each network upgrade, and every transaction must be signed for the branch in force. When `nextblock` differs from `chaintip`, an upgrade activates at the next block and signing software has to switch.

`backend.version` reads `zebra` with no version number. Read the exact node version from the JSON-RPC [getinfo](../zcash-json-rpc-api/getinfo-zcash.md) method instead.
{% endhint %}

## Use Cases

* **Health Checks**: Confirm the connection and the indexer are live
* **Sync Verification**: Compare `bestHeight` against an independent source
* **Upgrade Detection**: Watch `consensus.nextblock` differ from `chaintip` ahead of a network upgrade
* **Network Assertion**: Check `block0Hash` and `testnet` to confirm the chain

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
