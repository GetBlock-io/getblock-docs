---
description: >-
  This guide covers adding the network to a browser wallet, then deploying a
  simple contract using Foundry and Hardhat — pick whichever you prefer.
---

# Deploy a Smart Contract on Cronos

Cronos is EVM-compatible, so contracts deploy using the standard Solidity toolchain — Foundry, Hardhat, or Remix — with a GetBlock endpoint. This guide covers adding the network to a wallet and deploying a first contract to Cronos EVM.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with CRO on Cronos for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-cronos.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-cronos.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with CRO on Cronos

## Network Details

| Property        | Value                                                    |
| --------------- | -------------------------------------------------------- |
| Network Name    | Cronos EVM Mainnet                                       |
| RPC URL         | https://shared.eu-central-1.getblock.io/\<ACCESS-TOKEN>/ |
| Chain ID        | 25 (0x19)                                                |
| Currency Symbol | CRO                                                      |
| Block Explorer  | [explorer.cronos.org](https://explorer.cronos.org/)      |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Cronos is not, so it must be added manually — use the button below or the manual steps that follow.

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-cronos.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `25`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Cronos network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Cronos gas is paid in CRO.

* **Mainnet** — acquire CRO from an exchange or bridge assets in via the [Cronos Bridge](https://cronos.org/bridge), then send CRO to your deployer's address.
* **Testnet (Cronos Testnet 3, chain ID 338)** — request test CRO from the Cronos testnet faucet; see [docs.cronos.org](https://docs.cronos.org/) for the current faucet and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-cronos && cd hello-cronos
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Cronos";

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
export CRONOS_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $CRONOS_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify the source

The Cronos EVM Explorer supports source verification through Sourcify:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 25 \
  --verifier sourcify
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-cronos && cd hello-cronos
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

const CRONOS_CHAIN_ID = 25;

module.exports = {
  solidity: '0.8.24',
  networks: {
    cronos: {
      url: process.env.CRONOS_RPC_URL,
      chainId: CRONOS_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  sourcify: {
    enabled: true
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
    string public greeting = "Hello, Cronos";

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
export CRONOS_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network cronos
```
{% endstep %}

{% step %}
### Verify the source

```bash
npx hardhat verify --network cronos <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Cronoscan (the former Etherscan-style explorer) was deprecated on October 6, 2025. Use the Cronos EVM Explorer at [explorer.cronos.org](https://explorer.cronos.org/), which verifies contracts through Sourcify and Blockscout rather than an Etherscan API-key flow.
{% endhint %}
