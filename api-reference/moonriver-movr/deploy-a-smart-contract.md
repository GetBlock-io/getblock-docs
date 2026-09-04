# deploy a smart contract

Moonbeam is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint. This guide covers adding the network to a wallet and deploying a first contract to Moonbeam.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with GLMR on Moonbeam for gas — see [Add Network to Your Wallet](deploy-a-smart-contract.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with GLMR on Moonbeam

## Network Details

| Property        | Value                                                 |
| --------------- | ----------------------------------------------------- |
| Network Name    | Moonbeam                                              |
| RPC URL         | https://shared.eu-central-1.getblock.io//             |
| Chain ID        | 1284 (0x504)                                          |
| Currency Symbol | GLMR                                                  |
| Block Explorer  | [moonbeam.moonscan.io](https://moonbeam.moonscan.io/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Moonbeam is not, so it must be added manually — use the button below or the manual steps that follow.

{% hint style="info" %}
The button uses the EIP-3085 `wallet_addEthereumChain` request. Review the parameters before approving in your wallet.
{% endhint %}

{% code title="add-network-to-metamask.html" overflow="wrap" %}
```html
<button
    style="background: #6b7280; color: white; border: 0; padding: 10px 20px; border-radius: 6px; cursor: pointer; font-weight: 600;"
    onclick="addNetwork('0x504', 'Moonbeam', 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'https://moonbeam.moonscan.io')">
    Add Moonbeam
</button>

<script>
async function addNetwork(chainIdHex, chainName, rpcUrl, explorerUrl) {
  if (typeof window.ethereum === 'undefined') {
    alert('No EVM wallet detected. Install MetaMask or a compatible EIP-3085 wallet first.');
    return;
  }
  try {
    await window.ethereum.request({
      method: 'wallet_addEthereumChain',
      params: [{
        chainId: chainIdHex,
        chainName: chainName,
        rpcUrls: [rpcUrl],
        nativeCurrency: { name: 'Glimmer', symbol: 'GLMR', decimals: 18 },
        blockExplorerUrls: [explorerUrl]
      }]
    });
  } catch (err) {
    console.error('Failed to add network:', err);
    alert('Failed to add network: ' + (err.message || err));
  }
}
</script>
```
{% endcode %}

To add the network manually:

{% stepper %}
{% step %}
### Open your wallet

Open MetaMask (or your EVM wallet of choice).
{% endstep %}

{% step %}
### Add a custom network

Open the network selector and choose **Add a custom network**.
{% endstep %}

{% step %}
### Enter network details

Enter the values from the [Network Details](deploy-a-smart-contract.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `1284`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Moonbeam network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Moonbeam gas is paid in GLMR.

* **Mainnet** — acquire GLMR from an exchange or bridge, then send it to your deployer's address.
* **Testnet (Moonbase Alpha, chain ID 1287)** — request free DEV tokens from the Moonbase Alpha faucet; see [docs.moonbeam.network](https://docs.moonbeam.network/) for the current faucet and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-moonbeam && cd hello-moonbeam
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Moonbeam";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Deploy

```bash
export MOONBEAM_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $MOONBEAM_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Moonbeam uses a Moonscan (Etherscan-compatible) explorer:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 1284 \
  --verifier etherscan \
  --verifier-url https://api-moonbeam.moonscan.io/api \
  --etherscan-api-key <MOONSCAN_API_KEY>
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-moonbeam && cd hello-moonbeam
npm init --yes
npm install --save-dev hardhat
npx hardhat init
```
{% endstep %}

{% step %}
### Configure the network

{% code title="hardhat.config.js" %}
```javascript
require('@nomicfoundation/hardhat-toolbox');

const MOONBEAM_CHAIN_ID = 1284;

module.exports = {
  solidity: '0.8.24',
  networks: {
    moonbeam: {
      url: process.env.MOONBEAM_RPC_URL,
      chainId: MOONBEAM_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { moonbeam: process.env.MOONSCAN_API_KEY },
    customChains: [
      {
        network: 'moonbeam',
        chainId: MOONBEAM_CHAIN_ID,
        urls: {
          apiURL: 'https://api-moonbeam.moonscan.io/api',
          browserURL: 'https://moonbeam.moonscan.io'
        }
      }
    ]
  }
};
```
{% endcode %}
{% endstep %}

{% step %}
### Write the contract

{% code title="contracts/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Moonbeam";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Deploy

```bash
export MOONBEAM_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network moonbeam
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
export MOONSCAN_API_KEY=your_moonscan_api_key
npx hardhat verify --network moonbeam <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Moonbeam exposes Substrate features (staking, XCM, batching, native-asset ERC-20s) to Solidity through precompiles at fixed addresses in the `0x0000...08xx` range. See the Moonbeam documentation for each precompile's address and interface.
{% endhint %}

## After Deploying

* Read contract state through the [eth\_call](/broken/pages/e7544d76a0dd8b5960815a5b11252dca34339293) method against your GetBlock endpoint
* Watch contract events with [eth\_getLogs](/broken/pages/8289f6b1dcb652ca7ea68434b207eb1c9dbf6b16)
* Explore Moonbeam's precompiles to call staking, XCM, and batch operations from your contracts
