---
description: Step-by-step guide to getting a fast, reliable TON RPC endpoint
---

# How to Get a TON RPC Endpoint

The Open Network (TON) has become the leading blockchain for Telegram-integrated applications, mini-apps, and payments. With TON's unique architecture (TVM, not EVM) and the massive Telegram user base, demand for reliable TON RPC access is growing fast. Here's how to set up a TON endpoint with GetBlock.

### TON's API is Different from EVM Chains

TON uses its own HTTP API — not the JSON-RPC standard used by Ethereum and EVM chains. TON API methods include:

* `getAddressInformation` — get account balance and state
* `getTransactions` — fetch transaction history
* `detectAddress` — normalize address formats
* `estimateFee` — estimate transaction cost
* `sendBoc` — submit transactions

{% hint style="info" %}
GetBlock supports the full TON HTTP API specification.
{% endhint %}

### Step-by-Step: Get Your TON RPC Endpoint

{% stepper %}
{% step %}
#### Create a GetBlock Account

Go to [GetBlock Dashboard](https://account.getblock.io) and sign up. You can register with email or via Google/GitHub OAuth.&#x20;
{% endstep %}

{% step %}
#### Create a TON Endpoint

Once logged in:

1. Click **"Shared Nodes"** in the left sidebar
2. Click **"Create New Endpoint"** or the **"+"** button

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3.  Select:

    * **Protocol:** TON
    * **Network:** Mainnet
    * **API Interface:** JSON-RPC or JSON-RPC(v3) or HTTP API(v4)
    *   **Region:** Frankfurt (EU)

        <figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>


4. Click **"Create":** Your endpoint URL will be generated immediately.
{% endstep %}

{% step %}
#### Copy Your Endpoint URL

Your endpoint URL looks like this:

```
https://go.getblock.io/a1b2c3d4e5f6789012345678abcdef01/
```

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
curl "https://go.getblock.io/853387a2f5fb45938bc031b8dca61109/getTransactions?address=EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2&limit=10"
```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
{% code overflow="wrap" %}
```json
{
    "ok": true,
    "result": [
        {
            "@type": "raw.transaction",
            "address": {
                "@type": "accountAddress",
                "account_address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
            },
            "utime": 1776290803,
            "data": "te6cckECBgEAAWoAA7V+0WkTBwUARxF7mYtWHY3oLTH7+EkQztbrX8kudIXvinAABAWCj9eUOD7/OUrBX59ddg7e3dzvZoID+aKx+fen8wasJez5upJwAAQFZjZhbDaeAL8wABRhp1GIAQIDAQGgBACCch7Abw4H6w6slWcoil9zY7JBOborq1xVIKW99Tsth1ejdzUxO3jSz9uuy9Y1yGPJMw/Wr1dV7299L8x9tJpO574BIQSA7wjD0JAQYabaDQDBhqAQBQC5aAAkFnkPv5yCP72kT5+cJ0xbYR+O+oNCl+CYiPHg8WnI4QA7RaRMHBQBHEXuZi1YdjegtMfv4SRDO1utfyS50he+Kcw9CQAGCCNaAACAsFH68oTTwBfmAAAAAAVAAJxBDqgnEAAAAAB8AAAARgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAASDQZX",
            "transaction_id": {
                "@type": "internal.transactionId",
                "lt": "70747389000003",
                "hash": "02cNpr7ukamUPb7hxvpcD2X9o4wzFR/ur0m5tfh6bXM="
            },
            "fee": "866956",
            "storage_fee": "956",
            "other_fee": "866000",
            "in_msg": {
                "@type": "raw.message",
                "hash": "q36VnTxNsF94CFWxG9ACJ7WY/6hNABx9i2YSMW4in5I=",
                "source": "EQASCzyH385BH97SJ8_OE6YtsI_HfUGhS_BMRHjweLTkcOnQ",
                "destination": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
                "value": "1000000",
                "extra_currencies": [],
                "fwd_fee": "266669",
                "ihr_fee": "0",
                "created_lt": "70747389000002",
                "body_hash": "F99CaqBqWUd2tl2GzHTvFJZ3LcXszDAxo+ZkWq9ydzo=",
                "msg_data": {
                    "@type": "msg.dataText",
                    "text": "Cg=="
                },
                "message": "\n"
            },
            "out_msgs": []
        },
]
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
{% endstep %}
{% endstepper %}

### Code Example

{% tabs %}
{% tab title="JavaScript (tonweb)" %}
{% code overflow="wrap" %}
```javascript
import TonWeb from "tonweb";

const tonweb = new TonWeb(
  new TonWeb.HttpProvider("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/")
);

// Get balance
const balance = await tonweb.getBalance("EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2");
console.log(`Balance: ${TonWeb.utils.fromNano(balance)} TON`);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
```python
import requests

base_url = "https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"

# Get address info
response = requests.get(
    f"{base_url}getAddressInformation",
    params={"address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"}
)
data = response.json()
balance = int(data["result"]["balance"]) / 1e9
print(f"Balance: {balance} TON")
```
{% endtab %}
{% endtabs %}

### Why GetBlock for TON?

* **Reliable infrastructure** — 99.9%+ uptime vs unreliable public endpoints
* **No rate limiting drama** — up to 500 RPS vs public endpoint throttling
* **Global reach** — Frankfurt, New York, Singapore
* **Telegram Mini App scaling** — handle sudden user surges from Telegram viral growth

### Plans

| Use Case               | Plan                   | CU        | RPS       |
| ---------------------- | ---------------------- | --------- | --------- |
| Mini App prototype     | **Free**               | 50K/day   | 20        |
| Production Mini App    | **Starter** ($49/mo)   | 50M/mo    | 100       |
| Growing Telegram app   | **Advanced** ($199/mo) | 220M/mo   | 300       |
| Viral Telegram app     | **Pro** ($499/mo)      | 600M/mo   | 500       |
| Payment infrastructure | **Dedicated** (custom) | Unlimited | Unlimited |

[Learn more about our pricing](https://getblock.io/pricing/)

### What's Next?

* [Full TON API Reference →](https://docs.getblock.io/api-reference/the-open-network-ton)
* [TON Documentation →](https://docs.ton.org)
* [Configure a Dedicated TON Node →](https://getblock.io/dedicated-nodes/)

_Building a Telegram Mini App at scale?_ [_Contact us_](https://getblock.io/contact/) _for custom TON infrastructure._
