---
description: >-
  The getInflationReward JSON-RPC method retrieves staking rewards (inflation
  rewards) for a list of addresses during a specified epoch.
---

# getInflationReward – Solana

{% hint style="success" %}
The **getInflationReward** RPC Solana method provides reward distribution details for validators and stakers, including reward amount, effective slot, post-reward balance, and commission percentage (if applicable).&#x20;
{% endhint %}

The getInflationReward method retrieves **inflation rewards earned by one or more validator accounts for a given epoch**. It provides insights into staking rewards, helping delegators and validators track earnings and assess the impact of inflation-based incentives on the Solana network.

### Supported Networks

This method is accessible through Solana API endpoints:

* Mainnet
* Devnet

### Parameters

#### Optional Parameters

* **addresses** (`array`): A list of addresses as base-58 encoded strings.
* **commitment** (`string`): Defines the finality level of the response.
* **epoch** (`u64`): The epoch for which the reward is being queried (defaults to the previous epoch if omitted).
* **minContextSlot** (`number`): Ensures the request is evaluated at or after the specified slot.

### Request Example

#### API Endpoints

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### cURL Example

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/<ACCESS-TOKEN>/" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getInflationReward",
    "params": [
      [
        "6dmNQ5jwLeLk5REvio1JcMshcbvkYMwy26sJ8pbkvStu",
        "BGsqMegLpV6n6Ve146sSX2dTjUMj3M92HnU8BbNRMhF2"
      ],
      { "epoch": 2 }
    ]
}'

```
{% endtab %}
{% endtabs %}

### Response

A successful getInflationReward example response returns an array containing staking reward details.

#### Example Response

The result field is an array with elements corresponding (in order) to the list of addresses passed in the request.

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "amount": 2500,
      "effectiveSlot": 224,
      "epoch": 2,
      "postBalance": 499999442500
    },
    null
  ],
  "id": 1
}
```

#### **In this response:**

* `amount` (`u64`): The number of lamports rewarded during that epoch.&#x20;
* `effectiveSlot`(`u64`): The slot at which the reward was calculated.&#x20;
* `epoch`(`u64`): The epoch for which the reward was issued.&#x20;
* `postBalance`(`u64`): The account’s balance (in lamports) immediately after the reward was applied.
* `null`: The second address did not receive any reward for the requested epoch.

### Error Handling

Common getInflationReward error scenarios:

* Invalid address format: If an incorrect base-58 encoded address is provided.
* Epoch data unavailable: If the requested epoch has no recorded rewards.
* Network issues: API connectivity failures or request timeouts.

#### Example Error Response

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32000,
    "message": "Reward data unavailable for the given epoch"
  },
  "id": 1
}
```

### Use Cases

The Solana **getInflationReward** method is essential for:

* **Stakers and validators**: Tracking earned staking rewards per epoch;
* **Blockchain explorers**: Displaying staking reward history;
* **Web3 applications**: Providing insights into validator performance;
* **Analytics platforms**: Aggregating staking data for economic modeling.

### Code getInflationReward Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/"; 
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  id: 1,
  method: "getInflationReward",
  params: [
    [
      "6dmNQ5jwLeLk5REvio1JcMshcbvkYMwy26sJ8pbkvStu",
      "BGsqMegLpV6n6Ve146sSX2dTjUMj3M92HnU8BbNRMhF2"
    ],
    { "epoch": 2 }
  ]
};


const fetchInflationReward = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      console.log("Inflation Reward:", response.data.result);
      response.data.result.forEach((reward, index) => {
        console.log(`Account ${index + 1}:`);
        console.log(`  Epoch: ${reward.epoch}`);
        console.log(`  Effective Slot: ${reward.effectiveSlot}`);
        console.log(`  Amount: ${reward.amount}`);
        console.log(`  Post Balance: ${reward.postBalance}`);
        console.log(`  Commission: ${reward.commission ?? "N/A"}`);
      });
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getInflationReward error:", error.response?.data || error.message);
  }
};

fetchInflationReward();

```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the Web3 **getInflationReward** API with Solana’s Core API to track staking rewards dynamically. By leveraging JSON-RPC parameters and endpoints, developers can efficiently monitor staking performance, validator commissions, and epoch-based reward distributions.

\
