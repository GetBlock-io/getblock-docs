---
description: Learn how to deploy a smart contract on IOTA using GetBlock's RPC endpoint.
---

# Deploy a smart contract on IOTA

IOTA EVM is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in IOTA. This guide covers adding the network to a wallet and deploying a first contract to IOTA EVM.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with IOTA on IOTA EVM for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-iota.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-iota.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with IOTA on IOTA EVM

## Network Details

| Property        | Value                                                   |
| --------------- | ------------------------------------------------------- |
| Network Name    | IOTA EVM                                                |
| RPC URL         | https://shared.eu-central-1.getblock.io//               |
| Chain ID        | 8822 (0x2276)                                           |
| Currency Symbol | IOTA                                                    |
| Block Explorer  | [explorer.evm.iota.org](https://explorer.evm.iota.org/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. IOTA EVM is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-iota.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `8822`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added IOTA EVM network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

IOTA EVM gas is paid in IOTA.

* **Mainnet** — bridge IOTA into IOTA EVM through the official IOTA bridge, or acquire it on an exchange that supports IOTA EVM withdrawals, then send it to your deployer's address.
* **Testnet (IOTA EVM Testnet)** — claim test IOTA from the IOTA EVM Testnet faucet; see [docs.iota.org](https://docs.iota.org/) for the current chain ID, faucet, and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-iota && cd hello-iota
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, IOTA EVM";

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
export IOTA_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $IOTA_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

IOTA EVM's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 8822 \
  --verifier blockscout \
  --verifier-url https://explorer.evm.iota.org/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-iota && cd hello-iota
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

const IOTA_EVM_CHAIN_ID = 8822;

module.exports = {
  solidity: '0.8.24',
  networks: {
    iota: {
      url: process.env.IOTA_RPC_URL,
      chainId: IOTA_EVM_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { iota: 'empty' },
    customChains: [
      {
        network: 'iota',
        chainId: IOTA_EVM_CHAIN_ID,
        urls: {
          apiURL: 'https://explorer.evm.iota.org/api',
          browserURL: 'https://explorer.evm.iota.org'
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
    string public greeting = "Hello, IOTA EVM";

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
export IOTA_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network iota
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network iota <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
