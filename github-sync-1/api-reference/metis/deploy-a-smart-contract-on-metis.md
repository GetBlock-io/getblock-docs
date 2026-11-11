---
description: Learn how to deploy a smart contract on Metis using GetBlock's RPC endpoint.
---

# Deploy a Smart Contract on Metis

Metis is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in METIS. This guide covers adding the network to a wallet and deploying a first contract to Metis.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with METIS on Metis for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-metis.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-metis.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with METIS on Metis

## Network Details

| Property        | Value                                                               |
| --------------- | ------------------------------------------------------------------- |
| Network Name    | Metis Andromeda                                                     |
| RPC URL         | https://shared.eu-central-1.getblock.io//                           |
| Chain ID        | 1088 (0x440)                                                        |
| Currency Symbol | METIS                                                               |
| Block Explorer  | [andromeda-explorer.metis.io](https://andromeda-explorer.metis.io/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Metis is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-metis.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `1088`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Metis network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Metis gas is paid in METIS.

* **Mainnet** — bridge METIS into Metis Andromeda through the official Metis bridge, or acquire it on an exchange that supports Metis withdrawals, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-metis && cd hello-metis
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Metis";

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
export METIS_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $METIS_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Metis uses a Blockscout-based explorer, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 1088 \
  --verifier blockscout \
  --verifier-url https://andromeda-explorer.metis.io/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-metis && cd hello-metis
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

const METIS_CHAIN_ID = 1088;

module.exports = {
  solidity: '0.8.24',
  networks: {
    metis: {
      url: process.env.METIS_RPC_URL,
      chainId: METIS_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { metis: 'empty' },
    customChains: [
      {
        network: 'metis',
        chainId: METIS_CHAIN_ID,
        urls: {
          apiURL: 'https://andromeda-explorer.metis.io/api',
          browserURL: 'https://andromeda-explorer.metis.io'
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
    string public greeting = "Hello, Metis";

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
export METIS_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network metis
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network metis <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
