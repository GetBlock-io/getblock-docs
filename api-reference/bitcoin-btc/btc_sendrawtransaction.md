---
description: >-
  Example code for the sendrawtransaction JSON-RPC method. Complete guide on how
  to use sendrawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# sendrawtransaction - Bitcoin

This method submits a raw transaction (serialized, hex-encoded) to the local node and network.

### Parameters

| Parameter  | Type          | Required | Description                                                        |
| ---------- | ------------- | -------- | ------------------------------------------------------------------ |
| hexstring  | string        | Yes      | The hex string of the raw transaction.                             |
| maxfeerate | number/string | No       | Maximum feerate in BTC/kvB to reject transactions (default: 0.10). |

### Request

{% include "../../.gitbook/includes/curl-location-request-p... (1).md" %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07"
}
```

### Response Parameters

| Field  | Type   | Description                         |
| ------ | ------ | ----------------------------------- |
| result | string | The transaction hash (txid) in hex. |

### Use Case

The `sendrawtransaction` method is essential for:

* Broadcasting signed transactions to the network
* Wallet transaction submission
* Payment gateway implementations
* Automated payment systems
* DeFi protocol integrations
* Exchange withdrawal systems

### Error Handling

| Status Code | Error Message                      | Cause                                       |
| ----------- | ---------------------------------- | ------------------------------------------- |
| 403         | Forbidden                          | Missing or invalid ACCESS-TOKEN.            |
| -22         | TX decode failed                   | The transaction hex is malformed.           |
| -25         | Missing inputs                     | Transaction references non-existent inputs. |
| -26         | Insufficient fee                   | Transaction fee is below minimum relay fee. |
| -26         | TX rejected                        | Transaction violates consensus rules.       |
| -27         | Transaction already in block chain | Transaction was already confirmed.          |

### Integration With Web3

The `sendrawtransaction` method helps developers:

* Build wallet broadcast functionality
* Implement payment processing systems
* Create automated trading bots
* Support bulk transaction submission
* Enable real-time payment applications
