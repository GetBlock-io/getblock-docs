---
description: >-
  Example code for the pox REST method. Complete guide on how to use pox REST
  method in GetBlock Web3 documentation.
---

# pox - Stacks

Returns Proof-of-Transfer (PoX) details: the active PoX contract, cycle lengths, total liquid supply, and the current and next reward cycles with their stacking thresholds.

## Endpoint

```http
GET /v2/pox
```

## Example

{% code overflow="wrap" %}
```bash
export STACKS_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${STACKS_RPC}v2/pox"
```
{% endcode %}

## Response

```json
{
    "contract_id": "SP000000000000000000002Q6VF78.pox-4",
    "pox_activation_threshold_ustx": 68038426328817,
    "first_burnchain_block_height": 666050,
    "current_burnchain_block_height": 867890,
    "prepare_phase_block_length": 100,
    "reward_phase_block_length": 2000,
    "reward_slots": 4000,
    "total_liquid_supply_ustx": 1360768526576349,
    "current_cycle": {
        "id": 110,
        "min_threshold_ustx": 90000000000,
        "stacked_ustx": 500000000000000,
        "is_pox_active": true
    },
    "next_cycle": {
        "id": 111,
        "min_threshold_ustx": 90000000000,
        "blocks_until_prepare_phase": 150
    }
}
```

## Response Fields

| Field                             | Type   | Description                                                          |
| --------------------------------- | ------ | -------------------------------------------------------------------- |
| contract\_id                      | string | The active PoX contract (e.g. pox-4)                                 |
| current\_burnchain\_block\_height | number | Current Bitcoin block height                                         |
| reward\_phase\_block\_length      | number | Length of the reward phase in burn blocks                            |
| total\_liquid\_supply\_ustx       | number | Total liquid STX supply, in microSTX                                 |
| current\_cycle                    | object | The current reward cycle: id, threshold, stacked amount, active flag |
| next\_cycle                       | object | The next reward cycle and blocks until its prepare phase             |

## Use Cases

* **Stacking UX**: Show the current cycle, threshold, and time to the next cycle
* **Reward math**: Read cycle lengths to schedule stacking operations

## Error Handling

| Error                     | Message       | Description                                                                  |
| ------------------------- | ------------- | ---------------------------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid or the transaction was rejected |
| 404 / Not found           | Not found     | The account, contract, map entry, variable, or constant does not exist       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect                            |
