---
description: >-
  Learn how to deploy a smart contract on Etherlink using GetBlock's RPC
  endpoint.
---

# Deploy a smart contract on Etherlink

Etherlink is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in XTZ. This guide covers adding the network to a wallet and deploying a first contract to Etherlink.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with XTZ on Etherlink for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-etherlink.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-etherlink.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with XTZ on Etherlink

## Network Details

| Property        | Value                                                     |
| --------------- | --------------------------------------------------------- |
| Network Name    | Etherlink                                                 |
| RPC URL         | https://shared.eu-central-1.getblock.io//                 |
| Chain ID        | 42793 (0xa729)                                            |
| Currency Symbol | XTZ                                                       |
| Block Explorer  | [explorer.etherlink.com](https://explorer.etherlink.com/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Etherlink is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-etherlink.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `42793`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Etherlink network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Etherlink gas is paid in XTZ.

* **Mainnet** — bridge XTZ from Tezos L1 into Etherlink through the official Etherlink bridge, or acquire XTZ on an exchange that supports Etherlink withdrawals, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-etherlink && cd hello-etherlink
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Etherlink";

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
export ETHERLINK_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $ETHERLINK_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Etherlink's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 42793 \
  --verifier blockscout \
  --verifier-url https://explorer.etherlink.com/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-etherlink && cd hello-etherlink
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

const ETHERLINK_CHAIN_ID = 42793;

module.exports = {
  solidity: '0.8.24',
  networks: {
    etherlink: {
      url: process.env.ETHERLINK_RPC_URL,
      chainId: ETHERLINK_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { etherlink: 'empty' },
    customChains: [
      {
        network: 'etherlink',
        chainId: ETHERLINK_CHAIN_ID,
        urls: {
          apiURL: 'https://explorer.etherlink.com/api',
          browserURL: 'https://explorer.etherlink.com'
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
    string public greeting = "Hello, Etherlink";

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
export ETHERLINK_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network etherlink
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network etherlink <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
