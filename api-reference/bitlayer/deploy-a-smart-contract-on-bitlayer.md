---
description: >-
  Learn how to deploy a smart contract on Bitlayer using GetBlock's RPC
  endpoint.
---

# Deploy a smart contract on Bitlayer

Bitlayer is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in BTC. This guide covers adding the network to a wallet and deploying a first contract to Bitlayer.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with BTC on Bitlayer for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-bitlayer.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-bitlayer.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with BTC on Bitlayer

## Network Details

| Property        | Value                                       |
| --------------- | ------------------------------------------- |
| Network Name    | Bitlayer                                    |
| RPC URL         | https://shared.eu-central-1.getblock.io//   |
| Chain ID        | 200901 (0x310c5)                            |
| Currency Symbol | BTC                                         |
| Block Explorer  | [www.btrscan.com](https://www.btrscan.com/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Bitlayer is not, so it must be added manually — follow steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-bitlayer.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `200901`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Bitlayer network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Bitlayer gas is paid in BTC.

* **Mainnet** — bridge BTC into Bitlayer through the official Bitlayer bridge, or acquire it on an exchange that supports Bitlayer withdrawals, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-bitlayer && cd hello-bitlayer
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Bitlayer";

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
export BITLAYER_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $BITLAYER_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Bitlayer's BTR Scan explorer is Etherscan-compatible:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 200901 \
  --verifier etherscan \
  --verifier-url https://api.btrscan.com/api \
  --etherscan-api-key <BTRSCAN_API_KEY>
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-bitlayer && cd hello-bitlayer
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

const BITLAYER_CHAIN_ID = 200901;

module.exports = {
  solidity: '0.8.24',
  networks: {
    bitlayer: {
      url: process.env.BITLAYER_RPC_URL,
      chainId: BITLAYER_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { bitlayer: process.env.BTRSCAN_API_KEY },
    customChains: [
      {
        network: 'bitlayer',
        chainId: BITLAYER_CHAIN_ID,
        urls: {
          apiURL: 'https://api.btrscan.com/api',
          browserURL: 'https://www.btrscan.com'
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
    string public greeting = "Hello, Bitlayer";

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
export BITLAYER_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network bitlayer
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
export BTRSCAN_API_KEY=your_btrscan_api_key
npx hardhat verify --network bitlayer <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
