---
description: >-
  This page provides a guide to common JSON-RPC and HTTP errors when testing
  your connection with GetBlock's API.
---

# Errors and troubleshooting

### Connection issues

<table><thead><tr><th width="205">Code</th><th width="154">Error message</th><th>Solution</th></tr></thead><tbody><tr><td>401</td><td>Access denied</td><td>Double-check that <code>&#x3C;ACCESS_TOKEN></code> is correctly replaced with your actual token. Ensure there are no trailing spaces.</td></tr><tr><td>404</td><td>Could not resolve host</td><td>Verify that the URL <code>https://go.getblock.io/&#x3C;ACCESS_TOKEN>/</code> is correct.</td></tr><tr><td>429</td><td>Too many requests</td><td>Check your GetBlock account for usage limits. Upgrade your plan if necessary.</td></tr></tbody></table>

### JSON-RPC errors

<table><thead><tr><th width="205">Code</th><th width="158">Error message</th><th>Solution</th></tr></thead><tbody><tr><td>32601</td><td>The method does not exist/is not available</td><td>Verify the method name (<code>eth_blockNumber</code>, <code>eth_getBalance</code>, etc.) against the blockchain's JSON-RPC <a href="broken-reference">specifications</a>.</td></tr><tr><td>32602</td><td>Invalid argument</td><td>Ensure the parameters in the <code>params</code> array match the expected format for the method.</td></tr><tr><td>32700</td><td>Parse error</td><td>Double-check your JSON syntax. Ensure your request is properly formatted.</td></tr></tbody></table>
