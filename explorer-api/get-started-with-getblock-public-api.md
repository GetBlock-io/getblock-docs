---
hidden: true
---

# Get started with GetBlock public API

> If you are on this page - congratulations, you are only a few minutes away from diving in our developer API with 40,000 free requests every day. All you need to do is set up an account.

### How to set up an account?

To set up an account on GetBlock you need either click on the “Account” button in the upper right corner of the [main page](https://getblock.io/) or follow [this link](https://account.getblock.io/sign-in).

### Step 1. Signing up to GetBlock

Please, select a preferred sign-up option:

1. Connect wallet. By choosing this option, you are not required to share your email address and create a new password. If you do not currently have a wallet browser extension, you will be asked to install the extension.
2. Sign in with Google. Google will share your name, email address, language preference, and profile picture with getblock.io.
3. Sign up with email. You will be asked to provide your name and email address. Should you choose the first option, you’ll be asked to verify the email address you entered.

Note that to start using our service you will be asked to accept our [Terms of Service](https://getblock.io/terms-of-service/) and [Privacy Policy](https://getblock.io/privacy-policy/).

![screenshot 1](https://storage.getblock.io/web/docs/explorer-api/get-started/screenshot_1.webp)

### Step 2. Check your user ID.

Find your user ID located in the ‘Account Settings’ section. Please use it when contacting GetBlock’s team so we can identify your account and help you faster.

![screenshot 2](https://storage.getblock.io/web/docs/explorer-api/get-started/screenshot_2.webp)

### Step 3. Figuring out API access.

Select “NEAR Protocol” in the “Select Protocol”, then click “Mainnet” in the next “Select Network” tab. You can also find your API key from your account’s main page.

![screenshot 3](https://storage.getblock.io/web/docs/explorer-api/get-started/screenshot_3.webp)

### Step 4. Get to know request packages.

In the ‘Account settings’ section, you will find available pricing plans under the “Requests balance” button. You can also click “Add” in the top right corner of the account’s main page to see the plans. The free plan includes a daily limit of 40,000 requests per account. Unused requests cannot be transferred to the next day and are, therefore, lost.

When your application is accessing a certain blockchain (sends tokens, verifies network states, etc.) it is called a ‘request’. You can choose between ‘Launch’, ‘Build’, ‘Scale’, and ‘Unlimited’ options.

‘Launch’, ‘Build’, and ‘Scale’ plans are pay-per-use options. Purchased requests do not burn even if not used in 30 days.

Here is the calculation that helps to determine the time during which the whole amount of requests from a particular package will be spent at a maximum capacity set to 60 requests per second (RpS):

* **‘Launch’ pack**: 5,000,000 requests / (200 RpS \* 60 sec) = 416 min (or 6 hrs 57 min);
* **‘Build’ pack**: 10,000,000 requests / (200 RpS \* 60 sec) = 833 min (or 13 hrs 53 min);
* **‘Scale’ pack**: 50,000,000 requests / (200 RpS \* 60 sec) = 4166 min (or 69 hrs 27 min).

With the ‘Unlimited’ plan, users are given access to an unlimited number of requests on all dev APIs (now only NEAR available) for 30 days.

![screenshot 4](https://storage.getblock.io/web/docs/explorer-api/get-started/screenshot_4.webp)

### Step 5. Study GetBlock API documentation

You can see the “NEAR Protocol Explorer” tab on the left side menu. Go there and dive deeper into how GetBlock API works, which methods it uses.

![screenshot 5](https://storage.getblock.io/web/docs/explorer-api/get-started/screenshot_5.webp)

To use the API, you’ll need to paste your API key into the ‘header’ row that says ‘x-api-key: YOUR-API-KEY’.

### Step 6. Voila! You’re ready to use the API.

Once the docs are studied, let your dev journey begin! You have 40,000 free requests per day, and you can use them for anything. They are also renewed everyday, but the unused requests are not transferred to the next day. Enjoy!
