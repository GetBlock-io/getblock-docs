---
description: Step-by-step guide to getting a fast, reliable TRON RPC endpoint
---

# How to Get a TRON RPC Endpoint

TRON processes more USDT transfers than any other blockchain. If you're building payment systems, wallets, trading bots, or any application that touches TRC-20 tokens, you need a reliable TRON RPC endpoint. Here's how to set one up with GetBlock.

### Step-by-Step: Get Your TRON RPC Endpoint

{% stepper %}
{% step %}
#### Create a GetBlock Account

Go to [GetBlock Dashboard](https://account.getblock.io) and sign up. You can register with email or via Google/GitHub OAuth.&#x20;
{% endstep %}

{% step %}
#### Create a TRON Endpoint

Once logged in:

1. Click **"Shared Nodes"** in the left sidebar
2. Click **"Create New Endpoint"** or the **"+"** button

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3.  Select:

    * **Protocol:** TRON
    * **Network:** Mainnet or Testnet
    * **API Interface:** gRPC (Fullnode) or JSON-RPC (Fullnode) or REST (Fullnode) or gRPC (Solidity) or JSON-RPC (Solidity)
    * **Region:** Frankfurt (EU) or New York or Singapore

    <figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
4. Click **"Create":** Your endpoint URL will be generated immediately.
{% endstep %}

{% step %}
#### Copy Your Endpoint URL

Your endpoint URL looks like this:

{% code overflow="wrap" %}
```bash
https://go.getblock.io/a1b2c3d4e5f6789012345678abcdef01/
```
{% endcode %}

{% hint style="warning" %}
The long string after `go.getblock.io/` is your **access token** — keep it private.
{% endhint %}
{% endstep %}

{% step %}
#### Test the Connection

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://go.getblock.io/4ddcea01626040019722710f54259810/wallet/getaccount \
  -H "Content-Type: application/json" \
  -d '{"address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g", "visible": true}'
```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
{% code overflow="wrap" %}
```bash
```
{% endcode %}
{% endtab %}
{% endtabs %}
{% endstep %}
{% endstepper %}

### Code Sample

{% tabs %}
{% tab title="TronWeb (JavaScript)" %}
{% code overflow="wrap" %}
```javascript
import TronWeb from "tronweb";

const tronWeb = new TronWeb({
  fullHost: "https://go.getblock.io/<YOUR-ACCESS-TOKEN>/",
});

// Get TRX balance
const balance = await tronWeb.trx.getBalance("TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g");
console.log(`Balance: ${balance / 1e6} TRX`);

// Get USDT balance (TRC-20)
const contract = await tronWeb.contract().at("TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t"); // USDT
const usdtBalance = await contract.balanceOf("TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g").call();
console.log(`USDT: ${usdtBalance / 1e6}`);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import requests

url = "https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"

# Get latest block
response = requests.post(f"{url}wallet/getnowblock")
block = response.json()
print(f"Block: {block['block_header']['raw_data']['number']}")

# Get account info
response = requests.post(f"{url}wallet/getaccount", json={
    "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
    "visible": True
})
print(f"Balance: {response.json().get('balance', 0) / 1e6} TRX")
```
{% endcode %}
{% endtab %}
{% endtabs %}

### TRON API Interfaces

TRON supports multiple API interfaces. GetBlock offers:

| Interface             | Use Case                                         |
| --------------------- | ------------------------------------------------ |
| **HTTP API**          | Native TRON API — broadest method support        |
| **JSON-RPC**          | EVM-compatible interface for cross-chain tooling |
| **Solidity HTTP API** | Read-only confirmed data queries                 |

Most TRON developers use the HTTP API with TronWeb library.

### What's Next?

* [Full TRON API Reference ](https://docs.getblock.io/api-reference/tron-trx)
* [TronWeb Integration Guide](https://docs.getblock.io/guides/using-web3-libraries/connect-to-getblock-with-tronweb)
* [Dedicated Nodes ](https://getblock.io/dedicated-nodes/)
* [Learn more about our pricing](https://getblock.io/pricing/)

_Processing USDT at scale?_ [_Contact us_](https://getblock.io/contact/) _for dedicated TRON infrastructure._
