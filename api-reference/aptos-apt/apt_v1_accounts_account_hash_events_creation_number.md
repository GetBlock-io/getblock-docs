---
description: >-
  Example code for the /v1/accounts/{account_hash}/events/{creation_number}
  JSON-RPC method. Сomplete guide on how to use
  /v1/accounts/{account_hash}/events/{creation_number} json-rpc in GetBlock.io
  Web
---

# /v1/accounts/{account\_hash}/events/{creation\_number} - Aptos

This endpoint retrieves events for a given account based on the creation number of the event handle. Events are emitted during transactions and serve as logs of on-chain actions such as token transfers, deposits, or contract interactions.

## Supported Network

* Mainnet

***

## Parameters

| Parameter         | Data type | Description                                                                                             | Required | In    |
| ----------------- | --------- | ------------------------------------------------------------------------------------------------------- | -------- | ----- |
| `account_hash`    | string    | Aptos account address.                                                                                  | Yes      | Path  |
| `creation_number` | integer   | The creation number of the event handle to be retrieved.                                                | Yes      | Path  |
| `start`           | string    | Starting point or offset for retrieving events. Defaults to showing the latest transactions if omitted. | No       | Query |
| `limit`           | integer   | Maximum number of events to retrieve per request. Defaults to the standard page size if not provided.   | No       | Query |

***

## Request

**Base URL**

```bash
https://go.getblock.io/
<ACCESS_TOKEN>
```

**Request Example (cURL)**

```bash
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/events/0'
```

#### Response

```json
[
  {
    "version": "2665347733",
    "guid": {
      "creation_number": "0",
      "account_address": "0x190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e12"
    },
    "sequence_number": "1",
    "type": "0x1::account::CoinRegisterEvent",
    "data": {
      "type_info": {
        "account_address": "0xec42a352cc65eca17a9fa85d0fc602295897ed6b8b8af6a6c79ef490eb8f9eba",
        "module_name": "0x616d6d5f73776170",
        "struct_name": "0x506f6f6c4c6971756964697479436f696e3c3078313a3a6170746f735f636f696e3a3a4170746f73436f696e2c203078663232626564653233376130376531323162353664393161343931656237626364666431663539303739323661396535383333386639363461303162313766613a3a61737365743a3a555344543e"
      }
    }
  }
]

```

***

## Response Parameter Definition

| Field                  | Type   | Description                                                                |
| ---------------------- | ------ | -------------------------------------------------------------------------- |
| `version`              | string | Ledger version number at which the event was recorded.                     |
| `guid`                 | object | Globally unique identifier (GUID) for the event.                           |
| `guid.creation_number` | string | Creation number assigned to this event stream within the account.          |
| `guid.account_address` | string | Address of the account that owns this event handle.                        |
| `sequence_number`      | string | Sequential index of this event in the event stream.                        |
| `type`                 | string | Fully qualified event type (e.g., `0x1::account::WithdrawEvent`).          |
| `data`                 | object | Additional data payload associated with the event (depends on event type). |

## Use Cases

This method can be used for:

* Tracking account-level activity logs such as deposits, withdrawals, and transfers.
* Building blockchain explorers that display historical event data.
* Monitoring DeFi or NFT protocols that emit custom contract events.
* Triggering real-time alerts when specific on-chain events occur.

***

## Code Examples

### Node.js (Axios)

```js
import axios from 'axios';

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xc32f662cd9718f02d8a8e5628f8f642fa27cd9b5f457b406ed734901a4939e34/events/0?limit=1&start=1'
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});
```

### Python (Request)

```python
import requests

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xc32f662cd9718f02d8a8e5628f8f642fa27cd9b5f457b406ed734901a4939e34/events/0?limit=1&start=1"

response = requests.get(url)

print(response.text)
```

> Replace `<ACCESS_TOKEN>` with your actual GetBlock access token.

***

## Error Handling

| Status Code | Error Message           | Cause                                                   |
| ----------- | ----------------------- | ------------------------------------------------------- |
| **400**     | Invalid account address | The provided `account_hash` is invalid.                 |
| **401**     | Unauthorized            | Missing or invalid `<ACCESS_TOKEN>`.                    |
| **404**     | Event stream not found  | No event stream exists for the given `creation_number`. |
| **422**     | Invalid creation number | Malformed or unsupported creation number format.        |
| **500**     | Internal server error   | Node or network failure while retrieving events.        |

***

## Integration with Web3

By integrating `/v1/accounts/{account_hash}/events/{creation_number}`, developers can:

* Optimise event tracking for NFT, DeFi, and gaming applications.
* Build real-time activity feeds from on-chain actions.
* Map blockchain transactions into user-friendly notifications.
* Support advanced analytics for blockchain event visualisation.
