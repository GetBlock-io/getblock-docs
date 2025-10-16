---
description: >-
  Example code for the /v1/estimate_gas_price json-rpc method. Сomplete guide on
  how to use /v1/estimate_gas_price json-rpc in GetBlock.io Web3 documentation.
---

# /v1/estimate_gas_price

This endpoint gets the estimated gas price for executing a transaction in the Aptos blockchain network.

## Supported Network

* Mainnet

## Parameter

None

## Request

**Base URL**

```bash
https://go.getblock.io/<ACCESS_TOKEN>
```

**Example(cURL)***

```curl
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/estimate_gas_price'
```
## Response Example

```json
{
    "deprioritized_gas_estimate": 100,
    "gas_estimate": 100,
    "prioritized_gas_estimate": 150
}

```


## Response parameter definition

<table>
  <tr>
   <td><strong>Field</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>deprioritized_gas_estimate
   </td>
   <td>String
   </td>
   <td>Lower gas price estimate — slower inclusion, suitable for non-urgent txns.
   </td>
  </tr>
  <tr>
   <td>gas_estimate
   </td>
   <td>String
   </td>
   <td>Standard recommended gas price (balanced option).
   </td>
  </tr>
  <tr>
   <td>prioritized_gas_estimate
   </td>
   <td>String
   </td>
   <td>Higher gas price estimate — ensures faster transaction processing.
   </td>
  </tr>
</table>

## Use Cases

This method can be used for:

* Wallets can auto-suggest gas fees based on the user's urgency.

* dApps can provide a “fast/normal/slow” fee slider to users.

* Developers can programmatically ensure transactions won’t fail due to low gas.

* Cost optimisation for batch transactions.


## Code example

**Node(axios)**

```js
const axios = require('axios');

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/estimate_gas_price',
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});


```

**Python(Request)**


```python
import requests

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/estimate_gas_price"
response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)
```


## Error handling

<table>
  <tr>
   <td><strong>Status Code</strong>
   </td>
   <td><strong>Error Message</strong>
   </td>
   <td><strong>Cause</strong>
   </td>
  </tr>
  <tr>
   <td><strong>403</strong>
   </td>
   <td>Forbidden
   </td>
   <td>Missing or invalid &lt;ACCESS_TOKEN>.
   </td>
  </tr>
  <tr>
   <td><strong>500</strong>
   </td>
   <td>Internal server error
   </td>
   <td>Node or network failure when estimating gas.
   </td>
  </tr>
</table>

## Integration with Web3

By integrating `/v1/estimate_gas_price` into dApp, developers can:

* Provide real-time fee estimation inside wallets and dApps.
* Improve transaction confirmation rates by suggesting optimal gas.
* Enhance user experience with clear trade-offs (fast vs cheap).
* Avoid failed transactions due to underpriced gas.
