---
description: >-
  Learn how to set up custom RPC URL on MetaMask for faster, more reliable, and
  secure blockchain interactions.
icon: wallet
---

# Connect to GetBlock with MetaMask

MetaMask is a blockchain wallet available as a mobile application and a browser extension. It allows you to interact with Ethereum-based decentralized applications (dApps) directly from your browser.

{% hint style="success" %}
Using GetBlock nodes with MetaMask makes your blockchain experience faster and more reliable than using default RPCs:

* Connect directly to trusted nodes for secure transactions.
* Enjoy quicker transactions and smoother dApp interactions.
* Experience less downtime, even during busy network periods
{% endhint %}

This step-by-step tutorial will guide you through connecting GetBlock’s powerful nodes to your MetaMask wallet.

***

### Before you start

#### Set up your wallet

If you don’t already have MetaMask, download and install it from the official website: [https://metamask.io/download](https://metamask.io/download).

MetaMask extension is officially supported on Chrome, Brave, Firefox, Microsoft Edge, and Opera browsers.

#### Get a custom RPC URL

Generate a JSON-RPC URL for the selected network from your GetBlock [account](https://getblock.io/) and copy it.

<figure><img src="../.gitbook/assets/Docs_MM_Generate_RPC URL.svg" alt="How to get a custom RPC URL for MetaMask"><figcaption><p>Generate an RPC URL for MetaMask</p></figcaption></figure>

{% hint style="info" %}
Rename your endpoints to clearly indicate their purpose.
{% endhint %}

***

### Add GetBlock RPC to an existing network

If the network is already in your list but uses a default RPC provider, you can switch to GetBlock by following these steps:

1. Click on the current network name in MetaMask to open the dropdown of networks.
2. Find the network you want to edit and click the settings menu<img src="../.gitbook/assets/dots-vertical (1).svg" alt="" data-size="original">next to it. Select 'Edit'.

<figure><img src="../.gitbook/assets/Docs_MM_Add_1.svg" alt="How to set up the custom RPC URL on MetaMask"><figcaption></figcaption></figure>

3. Open the ‘**Default RPC URL**’ dropdown and select '**Add RPC URL'**.
4. Paste the JSON-RPC endpoint URL from your GetBlock account (`https://go.getblock.io/<ACCESS_TOKEN>/`).
5. Name the new RPC URL for easier identification.

<figure><img src="../.gitbook/assets/Docs_MM_Add_2.svg" alt="How to change the RPC URL on MetaMask"><figcaption></figcaption></figure>

6. Click 'Add URL', then 'Save' to confirm.

You can now switch between RPC URLs for that network as needed.

***

### Add a new network to MetaMask

If the network isn’t in your list, you can add it as a custom network. For this example, we will add the Polygon zkEVM to MetaMask.

1. Click on the current network name.
2. Select '**Add a custom network**'.

<figure><img src="../.gitbook/assets/Docs_MM_Add_New.svg" alt="Adding a new network to MetaMask using custom RPC URL"><figcaption></figcaption></figure>

3. Fill in the required fields:

<table><thead><tr><th width="147">Field</th><th width="256">Description</th><th width="353">Example</th></tr></thead><tbody><tr><td><strong>RPC URL</strong></td><td>The network's JSON-RPC URL from your GetBlock account</td><td><em><mark style="color:blue;">https://go.getblock.io/&#x3C;ACCESS_TOKEN>/</mark></em></td></tr><tr><td><strong>Chain ID</strong></td><td>The ID of the network</td><td><em><mark style="color:blue;">1101</mark></em></td></tr><tr><td><strong>Network name</strong></td><td>Fetched automatically based on Chain ID</td><td><em><mark style="color:blue;">Polygon zkEVM</mark></em></td></tr><tr><td><strong>Currency symbol</strong></td><td>Suggested automatically based on Chain ID</td><td><em><mark style="color:blue;">ETH</mark></em></td></tr><tr><td><strong>Block explorer URL</strong></td><td>Optional field</td><td><em><mark style="color:blue;">https://zkevm.polygonscan.com/</mark></em></td></tr></tbody></table>

4. Click 'Save', and the custom network will be added and selected as default.

{% hint style="info" %}
You can get the Chain ID of an EVM-compatible network (e.g. Polygon zkEVM) sending a request to your endpoint with this curl command:

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
    "id": "getblock.io"
  }'
```
{% endhint %}
