---
description: >-
  Example code for the suix_getStakesByIds JSON-RPC method. Complete guide on
  how to use suix_getStakesByIds JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getStakesByIds - Sui

This method returns stake details for specific StakedSui object IDs on the SUI network. Unlike `suix_getStakes` which returns all stakes for an address, this method retrieves data for specific stake objects when you already know their IDs.

## Parameters

| Parameter        | Type  | Required | Description                   |
| ---------------- | ----- | -------- | ----------------------------- |
| staked\_sui\_ids | array | Yes      | Array of StakedSui object IDs |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl.sh" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_getStakesByIds",
  "params": [
    [
      "0x378423de90ed03b694cecf443c72b5387b29a731d26d98108d7abc4902107d7d",
      "0x6a8e0f8fea6fda5488462e58724c034462b6064a08845e2ae2942fe7c4ee816d"
    ]
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'suix_getStakesByIds',
  params: [[
    '0x378423de90ed03b694cecf443c72b5387b29a731d26d98108d7abc4902107d7d',
    '0x6a8e0f8fea6fda5488462e58724c034462b6064a08845e2ae2942fe7c4ee816d'
  ]]
};

axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', payload)
.then(response => console.log(response.data));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "suix_getStakesByIds",
    "params": [[
        "0x378423de90ed03b694cecf443c72b5387b29a731d26d98108d7abc4902107d7d",
        "0x6a8e0f8fea6fda5488462e58724c034462b6064a08845e2ae2942fe7c4ee816d"
    ]]
}

response = requests.post("https://go.getblock.io/<ACCESS-TOKEN>/", json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "suix_getStakesByIds",
        "params": [[
            "0x378423de90ed03b694cecf443c72b5387b29a731d26d98108d7abc4902107d7d"
        ]]
    });
    let response = client.post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .json(&payload).send().await?;
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "validatorAddress": "0x3befb84f03a24386492bd3b05b1fd386172eb450e5059ce7df0ea6d9d6cefcaa",
      "stakingPool": "0x9a95cf69368e31b4dbe8ee9bdb3c0587bbc79d8fc6edf4007e185a962fd906df",
      "stakes": [
        {
          "stakedSuiId": "0x378423de90ed03b694cecf443c72b5387b29a731d26d98108d7abc4902107d7d",
          "stakeRequestEpoch": "62",
          "stakeActiveEpoch": "63",
          "principal": "200000000000",
          "status": "Active",
          "estimatedReward": "520000000"
        }
      ]
    }
  ],
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter        | Type   | Description                  |
| ---------------- | ------ | ---------------------------- |
| validatorAddress | string | Validator address            |
| stakingPool      | string | Staking pool ID              |
| stakedSuiId      | string | Stake object ID              |
| principal        | string | Staked amount                |
| status           | string | Active, Pending, or Unstaked |
| estimatedReward  | string | Estimated rewards            |

## Use Cases

* Query specific stake objects efficiently
* Track individual stake positions
* Verify stake object ownership
* Build stake management tools

## Error Handling

| Error Code | Description                    |
| ---------- | ------------------------------ |
| -32602     | Invalid params - malformed IDs |
| -32603     | Internal error - node issues   |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="ts-example.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const stakes = await client.getStakesByIds({
  stakedSuiIds: ['0x378423de90ed03b694cecf443c72b5387b29a731d26d98108d7abc4902107d7d']
});
console.log(stakes);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="rs-sdk-example.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let ids = vec!["0x378423de90ed03b694cecf443c72b5387b29a731d26d98108d7abc4902107d7d".parse()?];
    let stakes = sui.governance_api().get_stakes_by_ids(ids).await?;
    println!("{:?}", stakes);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
