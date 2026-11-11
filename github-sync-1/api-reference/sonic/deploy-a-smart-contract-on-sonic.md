---
description: >-
  This guide covers adding the network to a browser wallet, then deploying a
  simple contract using Foundry and Hardhat — pick whichever you prefer.
---

# Deploy A Smart contract on Sonic

Sonic is fully EVM-compatible, so contracts deploy with standard Ethereum tooling such as Foundry, Hardhat, and Remix. This guide covers adding Sonic to a wallet and deploying a first contract with Foundry and Hardhat through a GetBlock endpoint.

### Prerequisites

* A wallet holding the native S token for gas (see Add Network to Your Wallet below)
* A GetBlock access token from the GetBlock dashboard, used as `<ACCESS-TOKEN>` in the endpoint `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* Node.js version 20 or later, for Hardhat
* A funded deployer address, on Sonic Blaze Testnet for testing (see Testnet Faucets)

### Network Details

| Property        | Sonic Mainnet                            |
| --------------- | ---------------------------------------- |
| Chain ID        | 146 (0x92)                               |
| RPC URL         | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/` |
| Currency Symbol | S                                        |
| Block Explorer  | [sonicscan.org](https://sonicscan.org/)  |

{% hint style="info" %}
Sonic is not pre-configured in MetaMask by default. Only Ethereum Mainnet and Sepolia ship pre-configured, so Sonic must be added manually or through an add-network button before it appears in the wallet.
{% endhint %}

{% hint style="warning" %}
Every command below defaults to Mainnet — replace environment variable values&#x20;
{% endhint %}

{% hint style="warning" %}
Never commit a real private key. Use environment variables and prefer a throwaway deployer key for testing. Private keys committed to a public repo are drained within seconds by automated bots.
{% endhint %}

### Add Network to Your Wallet

{% stepper %}
{% step %}
#### Open your wallet

Open MetaMask, or the EVM wallet of choice.
{% endstep %}

{% step %}
#### Open the network menu

Click the network dropdown at the top of the wallet.
{% endstep %}

{% step %}
#### Add a network manually

Click **Add network**, then **Add a network manually**.
{% endstep %}

{% step %}
#### Enter network details

Enter the network details from the Prerequisites section above.
{% endstep %}

{% step %}
#### Save and switch networks

Save and switch to the newly added network.
{% endstep %}
{% endstepper %}

### Deploy with Foundry

{% stepper %}
{% step %}
#### Install Foundry

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```
{% endstep %}

{% step %}
#### Create a project

```bash
mkdir sonic-deploy && cd sonic-deploy
forge init
```
{% endstep %}

{% step %}
#### Create a contract

Create `src/HelloSonic.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract HelloSonic {
    function hello() external pure returns (string memory) {
        return "Hello from Sonic";
    }
}
```
{% endstep %}

{% step %}
#### Deploy

```bash
export PRIVATE_KEY=<your-deployer-private-key>
export SONIC_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

forge create src/HelloSonic.sol:HelloSonic \
  --rpc-url $SONIC_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
#### Verify on Block Explorer

SonicScan uses the Etherscan V2 unified API. Use the chain ID and a single Etherscan API key:

```bash
export ETHERSCAN_API_KEY=<your-etherscan-api-key>

forge verify-contract <contract_address> \
  src/HelloSonic.sol:HelloSonic \
  --chain-id 146 \
  --etherscan-api-key $ETHERSCAN_API_KEY
```
{% endstep %}
{% endstepper %}

### Deploy with Hardhat

{% stepper %}
{% step %}
#### Create a project

```bash
mkdir sonic-deploy && cd sonic-deploy
npm init -y
npm install --save-dev hardhat
npx hardhat init
```

Select a JavaScript project when prompted.
{% endstep %}

{% step %}
#### Configure networks

{% code title="hardhat.config.js" %}
```javascript
require('@nomicfoundation/hardhat-toolbox');

const PRIVATE_KEY = process.env.PRIVATE_KEY;

module.exports = {
  solidity: '0.8.26',
  networks: {
    sonic: {
      url: 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
      chainId: 146,
      accounts: [PRIVATE_KEY]
    },
  },
  etherscan: {
    apiKey: process.env.ETHERSCAN_API_KEY
  }
};
```
{% endcode %}
{% endstep %}

{% step %}
#### Create a contract

Create `contracts/HelloSonic.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract HelloSonic {
    function hello() external pure returns (string memory) {
        return "Hello from Sonic";
    }
}
```
{% endstep %}

{% step %}
#### Compile and deploy

Create `scripts/deploy.js`:

{% code title="scripts/deploy.js" %}
```javascript
const hre = require('hardhat');

async function main() {
  const contract = await hre.ethers.deployContract('HelloSonic');
  await contract.waitForDeployment();
  console.log('HelloSonic deployed to:', await contract.getAddress());
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
npx hardhat run scripts/deploy.js --network sonic
```
{% endstep %}

{% step %}
#### Verify on Block Explorer

```bash
npx hardhat verify --network sonic <contract_address>
```
{% endstep %}
{% endstepper %}
