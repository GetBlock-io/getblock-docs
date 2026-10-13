---
description: Learn how to deploy a smart contract on Lens using GetBlock's RPC endpoint.
---

# Deploy a smart contract on Lens

Lens is EVM-compatible, but it runs on the ZK Stack's EraVM rather than the standard EVM, so contracts must be compiled with the zkSync `zksolc` compiler — plain `solc` bytecode from vanilla Foundry or Hardhat will not run. In practice this means using the zkSync-aware toolchain (`hardhat-zksync` or `foundry-zksync`). Gas is paid in GHO. This guide covers adding the network to a wallet and deploying a first contract to Lens.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [foundry-zksync](https://docs.zksync.io/build/tooling/foundry)
* An EVM wallet (such as MetaMask) with GHO on Lens for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-lens.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-lens.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with GHO on Lens

## Network Details

| Property        | Value                                                    |
| --------------- | -------------------------------------------------------- |
| Network Name    | Lens Chain                                               |
| RPC URL         | https://shared.eu-central-1.getblock.io/\<ACCESS-TOKEN>/ |
| Chain ID        | 232 (0xe8)                                               |
| Currency Symbol | GHO                                                      |
| Block Explorer  | [explorer.lens.xyz](https://explorer.lens.xyz/)          |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Lens is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-lens.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `232`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Lens network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Lens gas is paid in GHO.

* **Mainnet** — bridge GHO (or the assets to obtain it) into Lens through the official Lens bridge, then send GHO to your deployer's address.
* **Testnet (Lens Testnet, chain ID 37111)** — obtain testnet funds from the Lens testnet faucet; see [lens.xyz](https://lens.xyz/) for the current faucet and endpoints.

## Deploy with Hardhat (zksync)

{% stepper %}
{% step %}
### Create the project and install the zksync plugins

```bash
mkdir hello-lens && cd hello-lens
npm init --yes
npm install --save-dev hardhat @matterlabs/hardhat-zksync zksync-ethers ethers
```
{% endstep %}

{% step %}
### Configure the network for the ZK Stack

{% code title="hardhat.config.js" %}
```javascript
require('@matterlabs/hardhat-zksync');

const LENS_CHAIN_ID = 232;

module.exports = {
  zksolc: { version: 'latest', settings: {} },
  networks: {
    lens: {
      url: process.env.LENS_RPC_URL,
      ethNetwork: 'mainnet',
      zksync: true,
      chainId: LENS_CHAIN_ID,
      verifyURL: 'https://explorer.lens.xyz/api'
    }
  },
  solidity: { version: '0.8.24' }
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
    string public greeting = "Hello, Lens";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Compile with zksolc and deploy

```bash
export LENS_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export WALLET_PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat compile
npx hardhat deploy-zksync --network lens
```
{% endstep %}
{% endstepper %}

## Deploy with foundry-zksync

{% stepper %}
{% step %}
### Install foundry-zksync

```bash
curl -L https://raw.githubusercontent.com/matter-labs/foundry-zksync/main/install-foundry-zksync | bash
foundryup-zksync
forge init hello-lens && cd hello-lens
```
{% endstep %}

{% step %}
### Deploy with the --zksync flag

```bash
export LENS_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --zksync \
  --rpc-url $LENS_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Lens uses a ZKsync-based block explorer with a verification API:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --zksync \
  --chain-id 232 \
  --verifier zksync \
  --verifier-url https://explorer.lens.xyz/api
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Lens Chain is the execution layer for the Lens social protocol. Beyond raw contract deployment, most Lens applications build against the Lens SDK and the social-graph contracts (profiles, posts, follows). Consult the Lens documentation for those higher-level integrations.
{% endhint %}
