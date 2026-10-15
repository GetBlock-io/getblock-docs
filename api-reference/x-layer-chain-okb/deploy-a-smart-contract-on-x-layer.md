---
description: Learn how to deploy a smart contract on X-Layer using GetBlock's RPC endpoint.
---

# Deploy a smart contract on X-Layer

X Layer is EVM-equivalent, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in OKB. This guide covers adding the network to a wallet and deploying a first contract to X Layer.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with OKB on X Layer for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-x-layer.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-x-layer.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with OKB on X Layer

## Network Details

| Property        | Value                                                  |
| --------------- | ------------------------------------------------------ |
| Network Name    | X Layer Mainnet                                        |
| RPC URL         | https://shared.eu-central-1.getblock.io//              |
| Chain ID        | 196 (0xc4)                                             |
| Currency Symbol | OKB                                                    |
| Block Explorer  | [www.oklink.com/xlayer](https://www.oklink.com/xlayer) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. X Layer is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-x-layer.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the chain ID

Confirm the Chain ID resolves to `196`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added X Layer network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

X Layer gas is paid in OKB.

* **Mainnet** — bridge OKB into X Layer through the official X Layer bridge, or acquire it on OKX, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-xlayer && cd hello-xlayer
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, X Layer";

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
export XLAYER_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $XLAYER_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

X Layer uses the OKLink explorer, which provides an Etherscan-compatible verification API:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 196 \
  --verifier etherscan \
  --verifier-url https://www.oklink.com/api/v5/explorer/xlayer/api \
  --etherscan-api-key <OKLINK_API_KEY>
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-xlayer && cd hello-xlayer
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

const XLAYER_CHAIN_ID = 196;

module.exports = {
  solidity: '0.8.24',
  networks: {
    xlayer: {
      url: process.env.XLAYER_RPC_URL,
      chainId: XLAYER_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { xlayer: process.env.OKLINK_API_KEY },
    customChains: [
      {
        network: 'xlayer',
        chainId: XLAYER_CHAIN_ID,
        urls: {
          apiURL: 'https://www.oklink.com/api/v5/explorer/xlayer/api',
          browserURL: 'https://www.oklink.com/xlayer'
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
    string public greeting = "Hello, X Layer";

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
export XLAYER_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network xlayer
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
export OKLINK_API_KEY=your_oklink_api_key
npx hardhat verify --network xlayer <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
