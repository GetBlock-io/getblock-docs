---
description: >-
    Example code for the /v1/accounts/{account_hash}/events/{event_handle}/{field_name} JSON-RPC method. Сomplete guide on how to use /v1/accounts/{account_hash}/events/{event_handle}/{field_name} json-rpc in GetBlock.io Web3 documentation.
---
    
  # /v1/accounts/{account_hash}/events/{event_handle}/{field_name}
    
 This endpoint retrieves events for a given account using an event handle struct and a field name within that struct. It allows more precise event queries compared to using only the creation number.
    
  
  ## Supported Network
    - Mainnet
    
  ## Parameters
    
  | Parameter      | Data type | Description                                                                                       | Required | In    |
  |----------------|-----------|---------------------------------------------------------------------------------------------------|----------|-------|
  | `account_hash` | string    | Aptos account address                                                                             | Yes      | Path  |
  | `event_handle` | string    | Struct type name that contains the event handle.                                                  | Yes      | Path  |
  | `field_name`   | string    | Field name within the struct that holds the event handle.                                         | Yes      | Path  |
  | `start`        | string    | Starting point or offset for retrieving events. Defaults to latest if not provided.               | No       | Query |
  | `limit`        | integer   | Maximum number of events to retrieve per request. Defaults to standard page size if unspecified.  | No       | Query |
    
    
## Request Example
    
**Base URL**
    
```bash
https://go.getblock.io/<ACCESS\_TOKEN>/
```
    
**Example (cURL)**
  ```bash
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/events/0x1::account::Account/coin_register_events'
 ```   

## Response Example

```bash
    [
      {
        "version": "2304959",
        "guid": {
          "creation_number": "0",
          "account_address": "0x190d44266241744264b964a37b8f09863167a12d3e70cda39376cfb4e3561e12"
        },
        "sequence_number": "0",
        "type": "0x1::account::CoinRegisterEvent",
        "data": {
          "type_info": {
            "account_address": "0x1",
            "module_name": "0x6170746f735f636f696e",
            "struct_name": "0x4170746f73436f696e"
          }
        }
      },
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
## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| guid.creation_number | string | Unique identifier for this event stream under the given account. |
| guid.account_address | string | Account address that owns this event handle. |
| sequence_number | string | Number of transactions submitted and committed on-chain by the account. |
| type | string | Type of event emitted. |


## Use Cases

This endpoint can be used to:

*   Query specific event handles such as `deposit_events` or `withdraw_events`.
    
*   Display transaction logs tied to particular smart contracts or account resources.
    
*   Track contract-specific activity within dApps for analytics or notifications.
    

  ## Code Examples

### Python (requests)

```python
    import requests
    
    url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/events/0x1::account::Account/coin_register_events"
    
    response = requests.get(url)
    
    print(response.text)
``` 
### Node.js (Axios)

```js
    import axios from 'axios';
    
    let config = {
      method: 'get',
      maxBodyLength: Infinity,
      url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/events/0x1::account::Account/coin_register_events'
    };
    
    axios.request(config)
    .then((response) => {
      console.log(JSON.stringify(response.data));
    })
    .catch((error) => {
      console.log(error);
    });
    
```
> Replace `<ACCESS_TOKEN>` with your actual GetBlock API token.
## Error Handling

| Status Code | Error Message | Cause |
| --- | --- | --- |
| 403 | Forbidden | Missing or invalid ACCESS_TOKEN. |
| 404 | Resource not found | Invalid event_handle or field_name. |
| 500 | Internal server error | Node or network issue — retry the request later. |


## Integration with Web3

By integrating `/v1/accounts/{account_hash}/events/{event_handle}/{field_name}`, developers can:

*   Track token-specific events like deposits, withdrawals, mints, or burns.
    
*   Enable in-wallet notifications (e.g., “You received 250 APT”).
    
*   Monitor DeFi or NFT protocol activity by subscribing to contract events.
    
*   Enhance analytics dashboards by focusing on targeted on-chain event streams.
