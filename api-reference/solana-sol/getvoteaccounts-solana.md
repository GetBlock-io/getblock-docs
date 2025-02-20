---
description: >-
  The getVoteAccounts JSON-RPC method retrieves detailed information about
  active vote accounts on the Solana network, including validators’ staking
  activity and performance metrics.
---

# getVoteAccounts - Solana

{% hint style="success" %}
The getVoteAccounts RPC method returns a list of active vote accounts, highlighting key parameters such as activatedStake, commission, and epochCredits.
{% endhint %}

As part of Solana’s Core API, it enables developers to monitor validator health, track decentralization metrics, and analyze governance-related value distributions.

getVoteAccounts RPC Solana method supports optional filters like votePubkey (to target specific validators) and keepUnstakedDelinquents (to include inactive validators). By leveraging these parameters, Web3 applications can tailor data retrieval for staking analytics or real-time network audits.

### Supported Networks

Access this method via Solana API endpoints:

* Mainnet
* Devnet

### Parameters

#### Config (object, optional)

Customize the request with these fields:

* commitment (string, optional): Confirmation level: finalized (default), confirmed, or processed.
* votePubkey (string, optional): Filter results to a specific validator’s vote address (Base58-encoded).
* keepUnstakedDelinquents (bool, optional): Include delinquent validators with no active stake (default: false).
* delinquentSlotDistance (u64, optional): Define the slot distance threshold for marking validators as delinquent.

### Request

#### API Endpoints

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

#### Example (cURL)

{% tabs %}
{% tab title="curl" %}
```json
curl --location "https://go.getblock.io/api-key" -XPOST \
--header "Content-Type: application/json" \
--data '{
    "jsonrpc": "2.0",
    "method": "getVoteAccounts",
    "params": [{"commitment": "finalized"}],
    "id": "getblock.io"
}'
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns two lists: current (active validators) and delinquent (underperforming validators). Each entry includes metrics like stake, commission, and voting history.

#### &#x20;getVoteAccounts example Response

```json
{
   "id": "getblock.io",
   "jsonrpc": "2.0",
   "result": {
       "current": [
           {
               "activatedStake": 65136133648061,
               "commission": 10,
               "epochCredits": [[280, 3946620, 3579916]],
               "epochVoteAccount": true,
               "lastVote": 122802577,
               "nodePubkey": "dpuDVLGSXT28Z3RGS28QBD5LUmcWARQVu36vXCEhhBg",
               "rootSlot": 122802546,
               "votePubkey": "6AqFc9V6PqyXJReuP12ATGggaVpG1Ppg4LFNvnqQYz8B"
           }
       ]
   }
}
```

#### Key Response Fields

* activatedStake: Stake delegated to the validator (in lamports).
* commission: Validator’s fee percentage.
* epochCredits: Voting credits earned per epoch.
* lastVote: Slot of the validator’s latest vote.

### Error Handling

Common getVoteAccounts error scenarios include:

* Invalid votePubkey format.
* Unsupported parameters (e.g., delinquentSlotDistance with non-standard values).
* Missing API key or incorrect endpoint.

#### Error Response Example

```json
{
   "jsonrpc": "2.0",
   "error": {
       "code": -32602,
       "message": "Invalid votePubkey format"
   },
   "id": "getblock.io"
}
```

### Use Case

The  Solana getVoteAccounts method powers:

* Staking platforms evaluating validator performance.
* Governance dashboards tracking voting participation.
* Analytics tools calculating network decentralization metrics.
* Block explorers displaying validator-specific transaction histories.

By filtering with votePubkey or analyzing epochCredits, developers build applications that enhance transparency in Solana’s block production ecosystem.

### Code getVoteAccounts Example – Web3 Integration

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/api-key";
const headers = { "Content-Type": "application/json" };

const payload = {
  jsonrpc: "2.0",
  method: "getVoteAccounts",
  params: [
    {
      commitment: "finalized",
      keepUnstakedDelinquents: false
    }
  ],
  id: "getblock.io"
};

const fetchVoteAccounts = async () => {
  try {
    const response = await axios.post(url, payload, { headers });

    if (response.status === 200 && response.data.result) {
      const { current, delinquent } = response.data.result;
      
      console.log("Current Validators:");
      current.forEach((account, index) => {
        console.log(`  Validator ${index + 1}:`);
        console.log(`    Vote Pubkey: ${account.votePubkey}`);
        console.log(`    Stake: ${account.activatedStake}`);
        console.log(`    Last Vote: ${account.lastVote}`);
        console.log(`    Commission: ${account.commission}%`);
      });

      if (delinquent.length > 0) {
        console.log("\nDelinquent Validators:");
        delinquent.forEach((account, index) => {
          console.log(`  Delinquent Validator ${index + 1}:`);
          console.log(`    Vote Pubkey: ${account.votePubkey}`);
          console.log(`    Stake: ${account.activatedStake}`);
          console.log(`    Last Vote: ${account.lastVote}`);
          console.log(`    Commission: ${account.commission}%`);
        });
      } else {
        console.log("\nNo delinquent validators found.");
      }
    } else {
      console.error("Unexpected response:", response.data);
    }
  } catch (error) {
    console.error("getVoteAccounts error:", error.response?.data || error.message);
  }
};

fetchVoteAccounts();
```
{% endtab %}
{% endtabs %}

### Integration with Web3

Integrate the Web3 getVoteAccounts RPC method into Web3 applications to monitor validator networks, optimize staking strategies, and ensure compliance with governance standards. By leveraging Core API parameters like commitment and votePubkey, developers deliver real-time insights into Solana’s decentralized value ecosystem.

\
