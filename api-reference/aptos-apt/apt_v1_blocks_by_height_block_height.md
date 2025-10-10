---
description: >-
  Example code for the /v1/blocks/by_height/{block_height} json-rpc method.
  Сomplete guide on how to use /v1/blocks/by_height/{block_height} json-rpc in
  GetBlock.io Web3 documentation.
---

# /v1/blocks/by_height/{block_height}

This endpoint gets a specific block's information from the Aptos blockchain network, given its height.

## Supported Networks

* Mainnet

## Parameter

<table>
  <tr>
   <td><strong>Parameter</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>In</strong>
   </td>
   <td><strong>Required</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td><code>block_height</code>
   </td>
   <td>Integer
   </td>
   <td>Path
   </td>
   <td>Yes
   </td>
   <td>Height (number) of the block to fetch.
   </td>
  </tr>
  <tr>
   <td><code>with_transactions</code>
   </td>
   <td>Boolean
   </td>
   <td>Query
   </td>
   <td>No
   </td>
   <td>If true, returns full transactions inside the block. Default: false.
   </td>
  </tr>
</table>

## Request

**Base URL**
```bash
https://go.getblock.io/<ACCESS_TOKEN>
```

**Example(cURL)**

```curl
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/blocks/by_height/425737645?with_transactions=false'
```

## Response Example

```json
{
    "block_height": "425737645",
    "block_hash": "0xe34a082b101823913815097bab2f211a15ba9db9578f4ff736ad01df1b28fb66",
    "block_timestamp": "1757259002201444",
    "first_version": "3363712571",
    "last_version": "3363712573",
    "transactions": null
}

```

## Response Parameter Definition
<table>
  <tr>
   <td><strong>Field</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>block_height
   </td>
   <td>The height of the block
   </td>
  </tr>
  <tr>
   <td>block_hash
   </td>
   <td>The hash of the block at the specified height
   </td>
  </tr>
  <tr>
   <td>block_timestamp
   </td>
   <td>The time at which the block was created/added to the chain
   </td>
  </tr>
  <tr>
   <td>first_version
   </td>
   <td>The version number of the first transaction in the block
   </td>
  </tr>
  <tr>
   <td>last_version
   </td>
   <td>The version number of the last transaction in the block
   </td>
  </tr>
  <tr>
   <td>transactions
   </td>
   <td>An array containing the details of the transactions included in the block
   </td>
  </tr>
  <tr>
   <td>type (transaction)
   </td>
   <td>The type of the change
   </td>
  </tr>
  <tr>
   <td>hash
   </td>
   <td>The hash of the transaction
   </td>
  </tr>
  <tr>
   <td>sender
   </td>
   <td>The account from which the transaction was sent
   </td>
  </tr>
  <tr>
   <td>sequence_number
   </td>
   <td>The sequence of a transaction sent by the specific sender
   </td>
  </tr>
  <tr>
   <td>max_gas_amount
   </td>
   <td>The maximum amount of gas allocated for the execution of a transaction
   </td>
  </tr>
  <tr>
   <td>gas_unit_price
   </td>
   <td>The cost per unit of gas (determines the transaction fee)
   </td>
  </tr>
  <tr>
   <td>expiration_timestamp_secs
   </td>
   <td>The timestamp until which the transaction can be included in a block
   </td>
  </tr>
  <tr>
   <td>payload
   </td>
   <td>The data carried by a transaction
   </td>
  </tr>
  <tr>
   <td>type (payload)
   </td>
   <td>The type of payload indicates the purpose of the data contained
   </td>
  </tr>
  <tr>
   <td>function
   </td>
   <td>The function associated with the payload
   </td>
  </tr>
  <tr>
   <td>type_arguments
   </td>
   <td>An array specifying the types of arguments provided to the function
   </td>
  </tr>
  <tr>
   <td>arguments
   </td>
   <td>An array containing the actual arguments passed to the function
   </td>
  </tr>
  <tr>
   <td>signature
   </td>
   <td>An array with signature details
   </td>
  </tr>
  <tr>
   <td>type (signature)
   </td>
   <td>The type of signature used to verify authenticity
   </td>
  </tr>
  <tr>
   <td>public_key
   </td>
   <td>The public key of the account that generated the signature
   </td>
  </tr>
  <tr>
   <td>signature (value)
   </td>
   <td>The actual signature generated with the private key
   </td>
  </tr>
</table>

## Use Cases
This method can be used for:
* Get a specific block by its height.
* Explore transactions inside a given block.
* Build block explorers or monitoring dashboards.

## Code Example

**Python(Request)**

```python
import requests

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/blocks/by_height/425737645?with_transactions=false"
response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)
```
**Node(Axios)**
```js
import axios from ‘axios’

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: "https://go.getblock.io/<ACCESS_TOKEN>/v1/blocks/by_height/425737645?with_transactions=false"
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});
```
## Error Handling

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
   <td>Invalid or missing ACCESS_TOKEN.
   </td>
  </tr>
  <tr>
   <td><strong>410</strong>
   </td>
   <td>Block has been pruned
   </td>
   <td>No block exists for the specified height or pruned.
   </td>
  </tr>
  <tr>
   <td><strong>500</strong>
   </td>
   <td>Internal server error
   </td>
   <td>Node or network issue; retry later.
   </td>
  </tr>
</table>


## Integration with Web3

By integrating /v1/blocks/by_height/{block_height}, developers can:
* __Synchronise chain data__ by fetching blocks sequentially.
* __Monitor on-chain activity__ at the block level.
* __Enable dApps to verify inclusion__ of transactions at a given height.
