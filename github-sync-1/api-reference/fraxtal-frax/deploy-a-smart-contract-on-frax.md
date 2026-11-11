---
description: Learn how to deploy a smart contract on Frax using GetBlock's RPC endpoint.
---

# Deploy a smart contract on Frax

Fraxtal is EVM-equivalent, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in FRAX. This guide covers adding the network to a wallet and deploying a first contract to Fraxtal.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with FRAX on Fraxtal for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-frax.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-frax.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with FRAX on Fraxtal

## Network Details

| Property        | Value                                     |
| --------------- | ----------------------------------------- |
| Network Name    | Fraxtal                                   |
| RPC URL         | https://shared.eu-central-1.getblock.io// |
| Chain ID        | 252 (0xfc)                                |
| Currency Symbol | FRAX                                      |
| Block Explorer  | [fraxscan.com](https://fraxscan.com/)     |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Fraxtal is not, so it must be added manually — follow the steps below:

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
### Enter the network details

Enter the values from the [Network Details](deploy-a-smart-contract-on-frax.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `252`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Fraxtal network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Fraxtal gas is paid in FRAX.

* **Mainnet** — bridge FRAX into Fraxtal through the official Frax bridge, or acquire it on an exchange that supports Fraxtal withdrawals, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-fraxtal && cd hello-fraxtal
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Fraxtal";

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
export FRAX_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $FRAX_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Fraxscan is an Etherscan-compatible explorer:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 252 \
  --verifier etherscan \
  --verifier-url https://api.fraxscan.com/api \
  --etherscan-api-key <FRAXSCAN_API_KEY>
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-fraxtal && cd hello-fraxtal
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

const FRAXTAL_CHAIN_ID = 252;

module.exports = {
  solidity: '0.8.24',
  networks: {
    fraxtal: {
      url: process.env.FRAX_RPC_URL,
      chainId: FRAXTAL_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { fraxtal: process.env.FRAXSCAN_API_KEY },
    customChains: [
      {
        network: 'fraxtal',
        chainId: FRAXTAL_CHAIN_ID,
        urls: {
          apiURL: 'https://api.fraxscan.com/api',
          browserURL: 'https://fraxscan.com'
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
    string public greeting = "Hello, Fraxtal";

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
export FRAX_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network fraxtal
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
export FRAXSCAN_API_KEY=your_fraxscan_api_key
npx hardhat verify --network fraxtal <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
