---
description: Learn how to deploy a smart contract on Gravity using GetBlock's RPC endpoint.
---

# Deploy a smart contract on Gravity

Gravity is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in G. This guide covers adding the network to a wallet and deploying a first contract to Gravity.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with G on Gravity for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-gravity.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-gravity.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with G on Gravity

## Network Details

| Property        | Value                                                 |
| --------------- | ----------------------------------------------------- |
| Network Name    | Gravity Alpha Mainnet                                 |
| RPC URL         | https://shared.eu-central-1.getblock.io//             |
| Chain ID        | 1625 (0x659)                                          |
| Currency Symbol | G                                                     |
| Block Explorer  | [explorer.gravity.xyz](https://explorer.gravity.xyz/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Gravity is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-gravity.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `1625`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Gravity network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Gravity gas is paid in G.

* **Mainnet** — acquire G from an exchange or bridge it into Gravity, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-gravity && cd hello-gravity
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Gravity";

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
export GRAVITY_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $GRAVITY_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Gravity's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 1625 \
  --verifier blockscout \
  --verifier-url https://explorer.gravity.xyz/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-gravity && cd hello-gravity
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

const GRAVITY_CHAIN_ID = 1625;

module.exports = {
  solidity: '0.8.24',
  networks: {
    gravity: {
      url: process.env.GRAVITY_RPC_URL,
      chainId: GRAVITY_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { gravity: 'empty' },
    customChains: [
      {
        network: 'gravity',
        chainId: GRAVITY_CHAIN_ID,
        urls: {
          apiURL: 'https://explorer.gravity.xyz/api',
          browserURL: 'https://explorer.gravity.xyz'
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
    string public greeting = "Hello, Gravity";

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
export GRAVITY_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network gravity
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network gravity <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
