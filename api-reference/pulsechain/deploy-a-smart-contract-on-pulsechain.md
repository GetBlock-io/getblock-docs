---
description: >-
  Learn how to deploy a smart contract on PulseChain using GetBlock's RPC
  endpoint.
---

# Deploy a smart contract on Pulsechain

PulseChain is EVM-identical, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in PLS. This guide covers adding the network to a wallet and deploying a first contract to PulseChain.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with PLS on PulseChain for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-pulsechain.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-pulsechain.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with PLS on PulseChain

## Network Details

| Property        | Value                                               |
| --------------- | --------------------------------------------------- |
| Network Name    | PulseChain                                          |
| RPC URL         | https://shared.eu-central-1.getblock.io//           |
| Chain ID        | 369 (0x171)                                         |
| Currency Symbol | PLS                                                 |
| Block Explorer  | [scan.pulsechain.com](https://scan.pulsechain.com/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. PulseChain is not, so it must be added manually — follow the steps below:

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
### Enter the network values

Enter the values from the [Network Details](deploy-a-smart-contract-on-pulsechain.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `369`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added PulseChain network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

PulseChain gas is paid in PLS.

* **Mainnet** — acquire PLS from an exchange or bridge, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-pulsechain && cd hello-pulsechain
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, PulseChain";

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
export PLS_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $PLS_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

PulseChain's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 369 \
  --verifier blockscout \
  --verifier-url https://scan.pulsechain.com/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-pulsechain && cd hello-pulsechain
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

const PULSECHAIN_CHAIN_ID = 369;

module.exports = {
  solidity: '0.8.24',
  networks: {
    pulsechain: {
      url: process.env.PLS_RPC_URL,
      chainId: PULSECHAIN_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { pulsechain: 'empty' },
    customChains: [
      {
        network: 'pulsechain',
        chainId: PULSECHAIN_CHAIN_ID,
        urls: {
          apiURL: 'https://scan.pulsechain.com/api',
          browserURL: 'https://scan.pulsechain.com'
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
    string public greeting = "Hello, PulseChain";

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
export PLS_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network pulsechain
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network pulsechain <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
