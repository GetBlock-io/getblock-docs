---
description: >-
  Learn how to deploy a smart contract on Electroneum using GetBlock's RPC
  endpoint.
---

# Deploy a smart contract on Electroneum

Electroneum Smart Chain is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in ETN. This guide covers adding the network to a wallet and deploying a first contract to Electroneum.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ETN on Electroneum for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-electroneum.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-electroneum.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ETN on Electroneum

## Network Details

| Property        | Value                                                                   |
| --------------- | ----------------------------------------------------------------------- |
| Network Name    | Electroneum Smart Chain                                                 |
| RPC URL         | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`               |
| Chain ID        | 52014 (0xcb2e)                                                          |
| Currency Symbol | ETN                                                                     |
| Block Explorer  | [blockexplorer.electroneum.com](https://blockexplorer.electroneum.com/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Electroneum is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-electroneum.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `52014`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Electroneum network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Electroneum gas is paid in ETN.

* **Mainnet** — acquire ETN from an exchange or the Electroneum app, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-etn && cd hello-etn
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Electroneum";

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
export ETN_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $ETN_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Electroneum's block explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 52014 \
  --verifier blockscout \
  --verifier-url https://blockexplorer.electroneum.com/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-etn && cd hello-etn
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

const ELECTRONEUM_CHAIN_ID = 52014;

module.exports = {
  solidity: '0.8.24',
  networks: {
    electroneum: {
      url: process.env.ETN_RPC_URL,
      chainId: ELECTRONEUM_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { electroneum: 'empty' },
    customChains: [
      {
        network: 'electroneum',
        chainId: ELECTRONEUM_CHAIN_ID,
        urls: {
          apiURL: 'https://blockexplorer.electroneum.com/api',
          browserURL: 'https://blockexplorer.electroneum.com'
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
    string public greeting = "Hello, Electroneum";

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
export ETN_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network electroneum
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network electroneum <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
