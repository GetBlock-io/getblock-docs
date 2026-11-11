---
description: >-
  Every error code the GetBlock Webhooks API returns, with its HTTP status and
  when it happens.
---

# Error Codes

### Error format

Every error answer from these endpoints has the same shape:

{% code overflow="wrap" %}
```json
{ "error": "cap_addresses_exceeded", "request_id": "…" }
```
{% endcode %}

`error` is a stable code to branch on; see the table below. Include `request_id` when you contact support.

### All codes

| HTTP | Code | When |
| --- | --- | --- |
| 400 | `malformed_body` | Invalid JSON, an unknown field, a wrong type, or `chain` / `network` / `trigger_type` in a `PATCH` |
| 400 | `empty_body` | A body is required |
| 400 | `invalid_trigger_type` | Unknown `trigger_type` |
| 400 | `trigger_type_not_supported` | `dropped_replaced` is not available |
| 400 | `invalid_phases` | `phases` empty or unknown |
| 400 | `confirm_depth_out_of_range` | Outside 2 to 96 on Ethereum |
| 400 | `unsupported_chain`, `unsupported_network` | Only `eth` / `mainnet` is accepted |
| 400 | `target_url_*` | The [target URL rules](../retries-and-endpoint-protection.md#target-url-rules) |
| 400 | `filter_required`, `filter_malformed`, `filter_not_object`, `filter_ambiguous_node`, `filter_empty_group`, `filter_empty_leaf`, `filter_invalid_leaf`, `filter_invalid_dir`, `filter_invalid_hex`, `filter_invalid_position`, `filter_invalid_list_ref`, `filter_too_deep`, `filter_too_large`, `filter_trigger_mismatch`, `filter_reserved_field`, `unknown_filter_leaf` | The filter tree is invalid (depth up to 8, up to 256 nodes) or does not suit the trigger. An unknown key in a filter node is `filter_invalid_leaf` in a leaf and `filter_malformed` next to `all` / `any` / `not` |
| 400 | `filter_would_be_empty` | The update would leave the webhook with no condition |
| 400 | `abi_filter_not_supported` | The `abi` leaf is not supported |
| 400 | `invalid_webhook_name`, `invalid_list_name` | Empty after trimming, over 128 bytes, or control characters |
| 400 | `list_name_taken` | You already have a list with this name |
| 400 | `invalid_batching` | Negative batching values |
| 400 | `invalid_plan_limits` | Your plan's limits are misconfigured. Contact support |
| 400 | `invalid_limit`, `invalid_cursor`, `invalid_status`, `invalid_from`, `invalid_to`, `invalid_time_range` | Query parameters of list and delivery-history requests |
| 401 | — | Missing or invalid API key |
| 403 | `plan_required` | A team account without a plan: create a webhook, create a list, resume a `paused_plan` webhook |
| 404 | `not_found` | No such webhook (or not yours); also an unknown path or method |
| 404 | `list_not_found` | No such address list (or not yours) |
| 409 | `cap_active_webhooks_exceeded` | The plan's active-webhook limit |
| 409 | `cap_addresses_exceeded` | The plan's addresses-per-webhook limit |
| 409 | `cap_account_addresses_exceeded` | The account-wide address limit |
| 409 | `cap_inline_addresses_exceeded`, `cap_list_refs_exceeded` | Addresses written into the filter / lists per webhook |
| 409 | `cap_addresses_per_list_exceeded`, `cap_address_lists_exceeded` | Addresses per list / lists per account |
| 409 | `list_in_use` | Deleting a list a webhook still references |
| 409 | `insufficient_balance` | Resuming a `paused_no_balance` webhook while the balance is empty |
| 409 | `webhook_not_active` | A test event for a webhook that is not `active` |
| 413 | `request_too_large` | The body is too large. Above about 5 MiB the gateway answers `413` itself, as plain text without a code |
| 500 | `internal` | An error on our side |
| 502 | — | A GetBlock service behind the API did not answer. Retry later |
| 503 | `plan_caps_unavailable` | Your plan could not be checked right now. Retry |
| 503 | `test_event_unavailable` | The test event was not confirmed in time; it may still arrive |
| 503 | `delivery_log_not_configured`, `database_not_configured` | The service is temporarily unavailable. Retry later |

Each endpoint page lists the codes that endpoint can return.
