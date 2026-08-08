---
description: >-
  This guide covers adding the network to a browser wallet, then deploying a
  simple contract using Foundry and Hardhat — pick whichever you prefer.
---

# Deploy Smart Contract on Unichain

Unichain is an EVM-equivalent OP Stack Layer 2 that runs standard EVM bytecode, so contracts deploy with the usual Ethereum tooling such as Foundry, Hardhat, and Remix without modification. This guide covers adding Unichain to a wallet and deploying a first contract with Foundry and Hardhat through a GetBlock endpoint.

## Prerequisites

* A wallet holding ETH on Unichain for gas (see Add Network to Your Wallet below)
* A GetBlock access token from the GetBlock dashboard, used as `<ACCESS-TOKEN>` in the endpoint `https://go.getblock.io/<ACCESS-TOKEN>/`
* Node.js version 20 or later, for Hardhat
* A funded deployer address, on Unichain Sepolia for testing (see Testnet Faucets)

### Network Details

| Property        | Unichain Mainnet                         | Unichain Sepolia Testnet                            |
| --------------- | ---------------------------------------- | --------------------------------------------------- |
| Chain ID        | 130 (0x82)                               | 1301 (0x515)                                        |
| RPC URL         | `https://go.getblock.io/<ACCESS-TOKEN>/` | `https://go.getblock.io/<ACCESS-TOKEN>/`            |
| Currency Symbol | ETH                                      | ETH                                                 |
| Block Explorer  | [uniscan.xyz](https://uniscan.xyz/)      | [sepolia.uniscan.xyz](https://sepolia.uniscan.xyz/) |

{% hint style="info" %}
Unichain is not pre-configured in MetaMask by default. Only Ethereum Mainnet and Sepolia ship pre-configured, so Unichain must be added manually or through an add-network button before it appears in the wallet.
{% endhint %}

{% hint style="warning" %}
Every command below defaults to Mainnet — replace environment variable values with testnet ones for a dry run before touching real funds.
{% endhint %}

{% hint style="warning" %}
Never commit a real private key. Use environment variables and prefer a throwaway deployer key for testing. Private keys committed to a public repo are drained within seconds by automated bots.
{% endhint %}

## Add Network to Your Wallet

{% stepper %}
{% step %}
### Open your wallet

Open MetaMask, or the EVM wallet of choice.
{% endstep %}

{% step %}
### Open network settings

Click the network dropdown at the top of the wallet.
{% endstep %}

{% step %}
### Add a network manually

Click **Add network**, then **Add a network manually**.
{% endstep %}

{% step %}
### Enter network details

Enter the network details from the Prerequisites section above.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added network.
{% endstep %}
{% endstepper %}

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```
{% endstep %}

{% step %}
### Create a project

```bash
mkdir unichain-deploy && cd unichain-deploy
forge init
```
{% endstep %}

{% step %}
### Create a contract

Create `src/HelloUnichain.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.30;

contract HelloUnichain {
    function hello() external pure returns (string memory) {
        return "Hello from Unichain";
    }
}
```
{% endstep %}

{% step %}
### Deploy

```bash
export PRIVATE_KEY=<your-deployer-private-key>
export UNICHAIN_RPC_URL=https://go.getblock.io/<ACCESS-TOKEN>/

forge create src/HelloUnichain.sol:HelloUnichain \
  --rpc-url $UNICHAIN_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on Block Explorer

Uniscan uses the Etherscan V2 unified API. Use the chain ID and a single Etherscan API key:

```bash
export ETHERSCAN_API_KEY=<your-etherscan-api-key>

forge verify-contract <contract_address> \
  src/HelloUnichain.sol:HelloUnichain \
  --chain-id 130 \
  --etherscan-api-key $ETHERSCAN_API_KEY
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create a project

```bash
mkdir unichain-deploy && cd unichain-deploy
npm init -y
npm install --save-dev hardhat
npx hardhat init
```

Select a JavaScript project when prompted.
{% endstep %}

{% step %}
### Configure networks

{% code title="hardhat.config.js" %}
```javascript
require('@nomicfoundation/hardhat-toolbox');

const PRIVATE_KEY = process.env.PRIVATE_KEY;

module.exports = {
  solidity: '0.8.30',
  networks: {
    unichain: {
      url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
      chainId: 130,
      accounts: [PRIVATE_KEY]
    },
    unichainSepolia: {
      url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
      chainId: 1301,
      accounts: [PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: process.env.ETHERSCAN_API_KEY
  }
};
```
{% endcode %}
{% endstep %}

{% step %}
### Create a contract

Create `contracts/HelloUnichain.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.30;

contract HelloUnichain {
    function hello() external pure returns (string memory) {
        return "Hello from Unichain";
    }
}
```
{% endstep %}

{% step %}
### Compile and Deploy

Create `scripts/deploy.js`:

{% code title="scripts/deploy.js" %}
```javascript
const hre = require('hardhat');

async function main() {
  const contract = await hre.ethers.deployContract('HelloUnichain');
  await contract.waitForDeployment();
  console.log('HelloUnichain deployed to:', await contract.getAddress());
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```
{% endcode %}

Then compile and deploy:

```bash
npx hardhat compile
npx hardhat run scripts/deploy.js --network unichain
```
{% endstep %}

{% step %}
### Verify on Block Explorer

```bash
npx hardhat verify --network unichain <contract_address>
```
{% endstep %}
{% endstepper %}

## Testnet Faucets

### Unichain Sepolia

* [Unichain Faucet](https://faucet.unichain.org/) — dispenses Unichain Sepolia ETH to a connected wallet (verify)
* [Superchain Faucet](https://console.optimism.io/faucet) — Sepolia ETH across OP Stack testnets, including Unichain Sepolia (verify)
* Bridge Sepolia ETH from Ethereum Sepolia through the [Unichain Bridge](https://www.unichain.org/bridge) as an alternative to faucets
