---
description: >-
  Example code for the suix_getLatestSuiSystemState JSON-RPC method. Complete
  guide on how to use suix_getLatestSuiSystemState JSON-RPC in GetBlock Web3
  documentation.
---

# suix\_getLatestSuiSystemState - Sui

This method returns the latest SUI system state object containing comprehensive network information. This includes current epoch, protocol version, reference gas price, total stake, validator set, and other critical system parameters.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_getLatestSuiSystemState",
  "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'suix_getLatestSuiSystemState',
  params: []
};

axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', payload)
.then(response => console.log(response.data));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="request.py" %}
```python
import requests

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "suix_getLatestSuiSystemState",
    "params": []
}

response = requests.post("https://go.getblock.io/<ACCESS-TOKEN>/", json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="request.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "suix_getLatestSuiSystemState",
        "params": []
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

{% code title="response.json" overflow="wrap" %}
```json
{
 "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "epoch": "1001",
        "protocolVersion": "104",
        "systemStateVersion": "2",
        "storageFundTotalObjectStorageRebates": "1387742117891600",
        "storageFundNonRefundableBalance": "886657447580865",
        "referenceGasPrice": "505",
        "safeMode": false,
        "safeModeStorageRewards": "0",
        "safeModeComputationRewards": "0",
        "safeModeStorageRebates": "0",
        "safeModeNonRefundableStorageFee": "0",
        "epochStartTimestampMs": "1767807788838",
        "epochDurationMs": "86400000",
        "stakeSubsidyStartEpoch": "20",
        "maxValidatorCount": "150",
        "minValidatorJoiningStake": "30000000000000000",
        "validatorLowStakeThreshold": "20000000000000000",
        "validatorVeryLowStakeThreshold": "15000000000000000",
        "validatorLowStakeGracePeriod": "7",
        "stakeSubsidyBalance": "317297380491000010",
        "stakeSubsidyDistributionCounter": "981",
        "stakeSubsidyCurrentDistributionAmount": "387420489000000",
        "stakeSubsidyPeriodLength": "90",
        "stakeSubsidyDecreaseRate": 1000,
        "totalStake": "7481459625179893908",
    "activeValidators": [
    {
     "suiAddress": "0x8f8ea04f3b751533db8b8da0a40eba1ca8332a92680f058d83b9459d061aaa54",
                "protocolPubkeyBytes": "gNhkGDfc1QPTb/j50fzMPmQVp8PCrSZlxd/b0Ec8nqf1N2rbMejspBhU3wi/ue2/AlgAH8g8alP6n3tNSqF9cBt5IevIYlERPvVxoekPWMWiGpLCB8NNm3iEwxKNwbgj",
                "networkPubkeyBytes": "MoHiJfYzs1i8+P6LYAXN/aO4CdjlpEptsuumFuLL9EM=",
                "workerPubkeyBytes": "oud9N1OOviX9yE3RJ2PqWyERYwGvUNaXC7aBhDRVeT4=",
                "proofOfPossessionBytes": "r7Kql1UwLivuv7AATUnZ2VejCWc21AoV+elG3gJU8U6aUab7cT6CUtFSpTC4z8sm",
                "name": "InfStones",
                "description": "InfStones runs an enterprise-grade node management and staking platform",
                "imageUrl": "https://github.com/sili-infstones/infstones-logo/raw/main/infstones_logo.webp",
                "projectUrl": "https://infstones.com",
                "netAddress": "/dns/prod.sui.infstones.io/tcp/8080/http",
                "p2pAddress": "/dns/prod.sui.infstones.io/udp/8084",
                "primaryAddress": "/dns/prod.sui.infstones.io/udp/8081",
                "workerAddress": "/dns/prod.sui.infstones.io/udp/8082",
                "nextEpochProtocolPubkeyBytes": null,
                "nextEpochProofOfPossession": null,
                "nextEpochNetworkPubkeyBytes": null,
                "nextEpochWorkerPubkeyBytes": null,
                "nextEpochNetAddress": null,
                "nextEpochP2pAddress": null,
                "nextEpochPrimaryAddress": null,
                "nextEpochWorkerAddress": null,
                "votingPower": "18",
                "operationCapId": "0x1074a602678c05b98fd0eb35982a6f18f25d37e177e59698375a6f04f89c435e",
                "gasPrice": "910",
                "commissionRate": "800",
                "nextEpochStake": "13736720334505164",
                "nextEpochGasPrice": "910",
                "nextEpochCommissionRate": "800",
                "stakingPoolId": "0x3b34ff964e2d7ead46b3fe377b17b11aad0eecda7b75c3afa6f4d9073be2f77e",
                "stakingPoolActivationEpoch": "0",
                "stakingPoolDeactivationEpoch": null,
                "stakingPoolSuiBalance": "13736715454850223",
                "rewardsPool": "653909686781330",
                "poolTokenBalance": "12682725122616071",
                "pendingStake": "0",
                "pendingTotalSuiWithdraw": "0",
                "pendingPoolTokenWithdraw": "0",
                "exchangeRatesId": "0xfc84a891514f2eb7f098457bf6451cb36886332116caaa52fce82249c9cabe48",
                "exchangeRatesSize": "999"
            }]
  },
  "pendingActiveValidatorsId": "0x719fdd5d050b2a1be364ab385ac3d163b7ac407e234721392d3c716a6332caf3",
        "pendingActiveValidatorsSize": "0",
        "pendingRemovals": [],
        "stakingPoolMappingsId": "0x3a4ec1afc6f550aa838aa4e823380a2c7c9567cf12e8e4dcc81ea7d411e544c8",
        "stakingPoolMappingsSize": "143",
        "inactivePoolsId": "0xf2dfc014f09869d512c7965d743e1f513853f492d9c7c0d755597154cb3ff8cd",
        "inactivePoolsSize": "15",
        "validatorCandidatesId": "0x94f89425db4d5bfa8d982d17f5746a1ac35ccb863662ee9486587e1e2d922763",
        "validatorCandidatesSize": "17",
        "atRiskValidators": [],
        "validatorReportRecords": [
            [
                "0x92c7bf9914897e8878e559c19a6cffd22e6a569a6dd4d26f8e82e0f2ad1873d6",
                [
                    "0x43b8f743162704af85214b0d0159fbef11aae0e996a8e9eac7fafda7fc5bd5f2",
                    "0xec73ec4d6b2a9403937b12ca625f7b3124c4459ff4e3caae6cf6376edefb9f3a"
                ]
            ]
        ]
}
```
{% endcode %}

## Response Parameters

| Parameter         | Type    | Description                     |
| ----------------- | ------- | ------------------------------- |
| epoch             | string  | Current epoch number            |
| protocolVersion   | string  | Current protocol version        |
| referenceGasPrice | string  | Current reference gas price     |
| totalStake        | string  | Total staked SUI                |
| epochDurationMs   | string  | Epoch duration in milliseconds  |
| activeValidators  | array   | List of active validators       |
| safeMode          | boolean | Whether network is in safe mode |

## Use Cases

* Display network status dashboards
* Monitor validator set changes
* Track staking economics
* Calculate epoch timing
* Build governance tools

## Error Handling

| Error Code | Description                    |
| ---------- | ------------------------------ |
| -32603     | Internal error - node issues   |
| -32000     | Server error - RPC unavailable |

## SDK Integration

{% tabs %}
{% tab title="Sui Typescript SDK" %}
{% code title="sdk.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const systemState = await client.getLatestSuiSystemState();
console.log(systemState);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDk" %}
{% code title="sdk.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let state = sui.governance_api().get_latest_sui_system_state().await?;
    println!("{:?}", state);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
