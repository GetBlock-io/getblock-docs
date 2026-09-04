# Deploy a Smart Contract on Harmony

Harmony's Shard 0 is EVM-compatible, so contracts deploy using the standard Solidity toolchain — Foundry, Hardhat, or Remix — with a GetBlock endpoint. This guide covers adding the network to a wallet and deploying a first contract to Harmony.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ONE on Harmony for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-harmony.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-harmony.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ONE on Harmony

## Network Details

| Property        | Value                                                 |
| --------------- | ----------------------------------------------------- |
| Network Name    | Harmony Mainnet (Shard 0)                             |
| RPC URL         | https://shared.eu-central-1.getblock.io//             |
| Chain ID        | 1666600000 (0x63564c40)                               |
| Currency Symbol | ONE                                                   |
| Block Explorer  | [explorer.harmony.one](https://explorer.harmony.one/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Harmony is not, so it must be added manually — use the button below or the manual steps that follow.

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-harmony.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `1666600000`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Harmony network.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Harmony accounts have two forms: a bech32 `one1...` address and an Ethereum `0x` address that map to the same account. MetaMask and the `eth_*` API use the `0x` form; convert between them with a Harmony SDK when working with native `hmy_*` tooling.
{% endhint %}

## Funding Your Deployer

Harmony gas is paid in ONE.

* **Mainnet** — acquire ONE from an exchange or bridge assets in via the [Harmony Bridge](https://bridge.harmony.one/), then send ONE to your deployer's `0x` address.
* **Testnet** — Harmony's testnet (Shard 0 test, chain ID 1666700000) has a public faucet; see [docs.harmony.one](https://docs.harmony.one/) for the current faucet and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-harmony && cd hello-harmony
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Harmony";

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
export HARMONY_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $HARMONY_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify the source

Harmony's explorer supports source verification through Sourcify:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 1666600000 \
  --verifier sourcify
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-harmony && cd hello-harmony
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

const HARMONY_CHAIN_ID = 1666600000;

module.exports = {
  solidity: '0.8.24',
  networks: {
    harmony: {
      url: process.env.HARMONY_RPC_URL,
      chainId: HARMONY_CHAIN_ID,
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
    string public greeting = "Hello, Harmony";

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
export HARMONY_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network harmony
```
{% endstep %}

{% step %}
### Verify the source

```bash
npx hardhat verify --network harmony <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
