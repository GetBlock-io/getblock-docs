---
description: Learn how to deploy a smart contract on Atleta using GetBlock's RPC endpoint.
---

# Deploy a Smart Contract on Atleta

Atleta is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint. This guide covers adding the network to a wallet and deploying a first contract to Atleta.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ATLA on Atleta for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-atleta.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-atleta.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ATLA on Atleta

## Network Details

| Property        | Value                                                    |
| --------------- | -------------------------------------------------------- |
| Network Name    | Atleta                                                   |
| RPC URL         | https://shared.eu-central-1.getblock.io/\<ACCESS-TOKEN>/ |
| Chain ID        | 2440 (0x988)                                             |
| Currency Symbol | ATLA                                                     |
| Block Explorer  | [scan.atleta.network](https://scan.atleta.network/)      |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Atleta is not, so it must be added manually — the manual steps that follow.

To add the network manually:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-atleta.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `2440`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Atleta network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Atleta gas is paid in ATLA.

* **Mainnet** — acquire ATLA from an exchange or the Atleta bridge, then send it to your deployer's address.
* **Testnet (Olympia, chain ID 2340)** — claim test ATLA from the Atleta Olympia faucet; the testnet RPC is `https://testnet-rpc.atleta.network` and the explorer is `https://blockscout.atleta.network`.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-atleta && cd hello-atleta
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Atleta";

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
export ATLETA_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $ATLETA_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify the source

Atleta's Blockscout explorer verifies contracts through Sourcify:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 2440 \
  --verifier sourcify
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-atleta && cd hello-atleta
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

const ATLETA_CHAIN_ID = 2440;

module.exports = {
  solidity: '0.8.24',
  networks: {
    atleta: {
      url: process.env.ATLETA_RPC_URL,
      chainId: ATLETA_CHAIN_ID,
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
    string public greeting = "Hello, Atleta";

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
export ATLETA_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network atleta
```
{% endstep %}

{% step %}
### Verify the source

```bash
npx hardhat verify --network atleta <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
