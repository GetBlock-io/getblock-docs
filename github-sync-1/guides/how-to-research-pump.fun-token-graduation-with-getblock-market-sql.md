---
description: >-
  Use GetBlock Market SQL to measure how many Pump.fun tokens graduate to
  PumpSwap, how fast they get there,  which launches graduate most often, and
  what their price does after migration.
icon: chart-mixed
---

# How to Research Pump.fun Token Graduation with Getblock Market SQL

Pump.fun launches tens of thousands of tokens a day, and only a small fraction ever fill their bonding curve and migrate to PumpSwap. How small is a matter of argument: figures quoted from public dashboards range from under 0.3% to around 1.4%, depending on the period and how "graduated" is counted.&#x20;

Answering it yourself normally means indexing token creations, decoding migration transactions, and replaying swaps — a pipeline that takes weeks to build before you can ask your first question. Live streams do not help either, because they show what is happening now, not what happened to last week's launches. And the question traders actually care about comes after graduation: what does the price do once the token reaches PumpSwap?

_In this guide, you will learn how to answer those questions with plain SQL over GetBlock's Market SQL Solana dataset: how many Pump.fun tokens graduate and how fast, which launches graduate most often, and what graduates' prices do in their first 24 hours on PumpSwap._

## What you'll build

A `research.py` script that, for one day of Pump.fun launches:

1. Places the study window far enough behind the newest data that every launch has had a full chance to graduate.
2. Counts launches and graduates, and computes the graduation rate and time-to-graduation percentiles server-side.
3. Breaks the graduation rate down by how many buys were bundled into the launch transaction.
4. Prices each graduate on PumpSwap at +5 minutes, +1 hour and +24 hours against the pool's opening price.
5. Prints a research report and saves four charts and two CSV files.

All filtering, joins and aggregation run on Market SQL. Your script receives a few summary rows and one row per graduate, never the raw swaps.

## Prerequisites

* **Python 3.9 or later**
* A [**GetBlock account**](https://account.getblock.io/)
* **Market SQL access to the Solana dataset**: the free 2-hour trial is enough. See [Getting Started](/broken/pages/330dffc2b9cc4575a09cae8a3e282e8a4e13ef94).
* Your **GetBlock user ID** and a **`gb_…` API key** from the [API Docs tab](https://account.getblock.io/products/sql#api-docs).
* Basic Python and SQL knowledge.

{% hint style="info" %}
Market SQL is read-only and built for research. For live Pump.fun activity, pair this guide with [How to Track Pump.fun Token Mints](how-to-track-pump.fun-token-mints-with-getblocks-yellowstone-grpc.md) and the [Pump.fun migrations listener](how-to-build-pump.fun-to-pumpswap-and-raydium-migrations-listener-with-getblock.md).
{% endhint %}

## Project Setup

{% stepper %}
{% step %}
### Create the project

```bash
mkdir pumpfun-graduation-research
cd pumpfun-graduation-research
python3 -m venv .venv
source .venv/bin/activate
```

Create `requirements.txt`:

{% code title="requirements.txt" %}
```
pandas>=2.0
matplotlib>=3.7
python-dotenv>=1.0
requests>=2.31
```
{% endcode %}

Install the dependencies:

```bash
pip install -r requirements.txt
```
{% endstep %}

{% step %}
### Add your credentials

Create a `.env` file:

{% code title=".env" overflow="wrap" %}
```bash
# Market SQL uses HTTP Basic Auth: your GetBlock user ID and a gb_… API key.
# Both are on the API Docs tab: https://account.getblock.io/products/sql#api-docs
GETBLOCK_USER_ID=
GETBLOCK_API_KEY=

# How many days of launches to study: 1 on the trial plans (10 s query limit), up to 30 on Monthly (60 s).
WINDOW_DAYS=1

# How long a launch is given to graduate, and how long the post-migration follow-up runs.
HORIZON_DAYS=2
```
{% endcode %}

{% hint style="warning" %}
Your API key grants access to every Market SQL dataset on your account. Add `.env` to `.gitignore` and never commit it.
{% endhint %}
{% endstep %}

{% step %}
### Connect from Python

Market SQL speaks the ClickHouse HTTP interface, so the client is a single authenticated `POST`, the same request as the cURL above. Create `client.py`:

{% code title="client.py" overflow="wrap" %}
```python
import io
import os

import pandas as pd
import requests
from dotenv import load_dotenv

load_dotenv()

MARKET_SQL_URL = "https://market-sql.eu-central-1.getblock.io"


class MarketSQL:
    """A minimal client for GetBlock Market SQL over the ClickHouse HTTP interface."""

    def __init__(self, database="solana"):
        user_id = os.getenv("GETBLOCK_USER_ID")
        api_key = os.getenv("GETBLOCK_API_KEY")
        if not user_id or not api_key:
            raise SystemExit("Set GETBLOCK_USER_ID and GETBLOCK_API_KEY in .env")
        self.database = database
        self.session = requests.Session()
        self.session.auth = (user_id, api_key)  # HTTP Basic Auth

    def query_df(self, sql, params=None):
        """Run one read-only SQL statement and return the rows as a DataFrame.

        `params` fill ClickHouse placeholders such as {start:DateTime}. They are
        sent as `param_<name>` URL parameters, separately from the SQL text.
        """
        url_params = {"database": self.database, "default_format": "CSVWithNames"}
        for name, value in (params or {}).items():
            url_params[f"param_{name}"] = str(value)

        response = self.session.post(
            MARKET_SQL_URL,
            params=url_params,
            data=sql.encode(),
            timeout=120,
        )
        if response.status_code != 200:
            raise RuntimeError(f"Market SQL error {response.status_code}: {response.text.strip()}")
        if not response.text.strip():
            return pd.DataFrame()
        return pd.read_csv(io.StringIO(response.text))


def get_client():
    return MarketSQL()
```
{% endcode %}

The client asks for `CSVWithNames`, which `pandas.read_csv` turns straight into a DataFrame with numeric columns already typed.

Then create `inspect_tables.py`, which prints the columns of the three tables so you can see exactly what you are querying:

{% code title="inspect_tables.py" overflow="wrap" %}
```python
"""Print the columns of every table this research uses.

Run this first: it confirms your credentials work and shows the exact
column names the queries in queries.py rely on.
"""
from client import get_client

TABLES = [
    "pumpfun_token_creation",
    "pfamm_migrations",
    "pumpswap_all_swaps",
]


def main():
    client = get_client()
    for table in TABLES:
        columns = client.query_df(f"DESCRIBE TABLE {table}")
        print(f"\n{table} ({len(columns)} columns)")
        for _, col in columns.iterrows():
            print(f"  {col['name']:<36} {col['type']}")


if __name__ == "__main__":
    main()
```
{% endcode %}

Run it:

```bash
python inspect_tables.py
```

```
pumpfun_token_creation (41 columns)
  block_time                           DateTime
  slot                                 UInt32
  tx_idx                               UInt16
  creator                              FixedString(48)
  name                                 FixedString(20)
  symbol                               FixedString(16)
  url                                  FixedString(256)
  mint                                 FixedString(48)
  ...
```
{% endstep %}

{% step %}
### Write the queries

Create `queries.py`. Every query starts from the same two building blocks:

* **`launches`**: every token created inside the study window.
* **`graduates`**: the launches that appear in `pfamm_migrations` within the horizon, with the price their PumpSwap pool opened at.

Each research query appends its own `SELECT` to that shared `WITH` block, so the cohort is defined once.

{% code title="queries.py" overflow="wrap" %}
```python
"""SQL for the graduation research, run server-side on Market SQL.

Every query is bounded by {start}/{end} on block_time so it finishes inside
the execution limit of your plan (10 seconds on the trials).
"""

WSOL = "So11111111111111111111111111111111111111112"

# Newest launch in the dataset: the study window is placed behind it.
LATEST_LAUNCH = "SELECT max(block_time) AS latest FROM pumpfun_token_creation"

# Shared building blocks:
#   launches   - every Pump.fun token created inside the window
#   graduates  - the launches that migrated to PumpSwap within the horizon,
#                with the pool's opening price (lamports per raw token unit)
COHORT = """
WITH
launches AS (
    -- mint is a FixedString(48) here; toString() makes it joinable with String mints
    SELECT
        toString(mint) AS mint,
        min(block_time) AS created_at,
        max(bundled_buys_count) AS bundled_buys
    FROM pumpfun_token_creation
    WHERE block_time >= {start:DateTime} AND block_time < {end:DateTime}
    GROUP BY mint
),
graduates AS (
    SELECT
        mint,
        min(block_time) AS migrated_at,
        argMin(sol_amount / mint_amount, (slot, tx_idx)) AS migration_price
    FROM pfamm_migrations
    WHERE block_time >= {start:DateTime}
      AND block_time < {end:DateTime} + toIntervalDay({horizon:UInt16})
      AND mint IN (SELECT mint FROM launches)
    GROUP BY mint
)
"""

# 1. The funnel: how many launched, how many graduated, how fast.
FUNNEL = COHORT + """
SELECT
    count() AS launches,
    countIf(g.mint != '') AS graduated,
    round(graduated / launches * 100, 3) AS graduation_rate_pct,
    quantileIf(0.5)(dateDiff('minute', l.created_at, g.migrated_at), g.mint != '')
        AS median_minutes_to_graduate,
    quantileIf(0.9)(dateDiff('minute', l.created_at, g.migrated_at), g.mint != '')
        AS p90_minutes_to_graduate
FROM launches AS l
LEFT JOIN graduates AS g ON g.mint = l.mint
"""

# 2. One row per graduate, for the time-to-graduation chart.
GRADUATES = COHORT + """
SELECT
    l.mint AS mint,
    l.created_at AS created_at,
    g.migrated_at AS migrated_at,
    dateDiff('minute', l.created_at, g.migrated_at) AS minutes_to_graduate
FROM launches AS l
INNER JOIN graduates AS g ON g.mint = l.mint
ORDER BY minutes_to_graduate
"""

# 3. Do launches with buys bundled into the creation transaction graduate
#    more often than launches without?
BY_BUNDLE = COHORT + """
SELECT
    multiIf(l.bundled_buys = 0, '0',
            l.bundled_buys = 1, '1',
            l.bundled_buys <= 4, '2-4',
            '5+') AS bundled_buys,
    count() AS launches,
    countIf(g.mint != '') AS graduated,
    round(graduated / launches * 100, 2) AS graduation_rate_pct
FROM launches AS l
LEFT JOIN graduates AS g ON g.mint = l.mint
GROUP BY bundled_buys
ORDER BY bundled_buys
"""

# 4. What happened next: each graduate's last traded price on PumpSwap at
#    +5 min, +1 h and +24 h after migration, against the pool's opening price.
#    A PumpSwap pool can list the token on either side, so prices are
#    normalised to lamports per raw token unit whichever side WSOL is on.
POST_MIGRATION = COHORT + """
, swaps AS (
    SELECT
        block_time,
        slot,
        tx_idx,
        if(base_token = '{wsol}', quote_token, base_token) AS mint,
        if(base_token = '{wsol}',
           base_token_amount / quote_token_amount,
           quote_token_amount / base_token_amount) AS price
    FROM pumpswap_all_swaps
    WHERE block_time >= {start:DateTime}
      AND block_time < {end:DateTime} + toIntervalDay({horizon:UInt16} + 1)
      AND (base_token IN (SELECT mint FROM graduates)
           OR quote_token IN (SELECT mint FROM graduates))
      AND base_token_amount > 0 AND quote_token_amount > 0
)
SELECT
    g.mint AS mint,
    g.migrated_at AS migrated_at,
    g.migration_price AS migration_price,
    count() AS swaps_24h,
    argMaxIf(s.price, (s.slot, s.tx_idx),
             s.block_time <= g.migrated_at + INTERVAL 5 MINUTE) AS price_5m,
    argMaxIf(s.price, (s.slot, s.tx_idx),
             s.block_time <= g.migrated_at + INTERVAL 1 HOUR) AS price_1h,
    argMax(s.price, (s.slot, s.tx_idx)) AS price_24h,
    max(s.block_time) AS last_swap
FROM swaps AS s
INNER JOIN graduates AS g ON g.mint = s.mint
WHERE s.block_time >= g.migrated_at
  AND s.block_time < g.migrated_at + INTERVAL 1 DAY
GROUP BY g.mint, g.migrated_at, g.migration_price
""".replace("{wsol}", WSOL)
```
{% endcode %}

A few details worth noticing:

* **`toString(mint)`.** `pumpfun_token_creation` stores mints as `FixedString(48)`, while `pfamm_migrations` and `pumpswap_all_swaps` use `String`. Converting once in `launches` makes every later join and `IN` filter compare like with like.
* **Time bounds on every table.** Each scan is limited by `block_time`. Market SQL tables hold a year or more of Solana activity, so an unbounded scan of `pumpswap_all_swaps` can run for minutes.
* **`IN (SELECT mint FROM …)`.** Filtering migrations and swaps down to the cohort before joining keeps the right-hand side of every join small.
* **The pool's opening price as baseline.** At migration, `sol_amount` lamports and `mint_amount` raw tokens seed the PumpSwap pool, so `sol_amount / mint_amount` is the price every first buyer faced. Dozens of snipers often trade in the same second as the migration, which makes "the first swap" an unstable baseline.
* **Ordering by `(slot, tx_idx)`.** `block_time` has one-second resolution. `argMaxIf(price, (slot, tx_idx), …)` picks the last trade before each checkpoint by its exact position on-chain.
* **Price orientation.** A PumpSwap pool can hold the token as either base or quote. The query checks which side is WSOL and always returns lamports per raw token unit, the same unit as the migration price. Decimals cancel out because only ratios between prices are used.
{% endstep %}

{% step %}
### Build the research script

Create `research.py`:

{% code title="research.py" overflow="wrap" %}
```python
"""Pump.fun graduation research on GetBlock Market SQL.

For a window of Pump.fun launches, answers:
  1. How many tokens graduate to PumpSwap, and how fast?
  2. Do launches with bundled buys graduate more often?
  3. What happens to a graduate's price after migration?
"""
import os
from datetime import timedelta
from pathlib import Path

import matplotlib
import pandas as pd

matplotlib.use("Agg")
import matplotlib.pyplot as plt  # noqa: E402

import queries  # noqa: E402
from client import get_client  # noqa: E402

OUT = Path("out")
WINDOW_DAYS = int(os.getenv("WINDOW_DAYS", "1"))
HORIZON_DAYS = int(os.getenv("HORIZON_DAYS", "2"))
CHECKPOINTS = ("5m", "1h", "24h")


def study_window(client):
    """End the window HORIZON_DAYS before the newest data, so every launch
    in it has had the full horizon to graduate."""
    latest = pd.Timestamp(client.query_df(queries.LATEST_LAUNCH)["latest"].iloc[0])
    end = latest.normalize() - timedelta(days=HORIZON_DAYS)
    start = end - timedelta(days=WINDOW_DAYS)
    return start, end


def post_migration_returns(client, params):
    df = client.query_df(queries.POST_MIGRATION, params)
    for label in CHECKPOINTS:
        price = df[f"price_{label}"].where(df[f"price_{label}"] > 0)  # 0 = no trade yet
        df[f"return_{label}_pct"] = (price / df["migration_price"] - 1) * 100
    return df


def print_report(start, end, funnel, bundles, returns):
    print(f"\nWindow: launches from {start:%Y-%m-%d %H:%M} to {end:%Y-%m-%d %H:%M} UTC")
    print(f"Horizon: {HORIZON_DAYS} days to graduate, 24 h of trading after migration\n")

    print("1. Graduation funnel")
    print(f"   Launches          {int(funnel['launches']):>10,}")
    print(f"   Graduated         {int(funnel['graduated']):>10,}")
    print(f"   Graduation rate   {funnel['graduation_rate_pct']:>10.2f} %")
    print(f"   Median time       {funnel['median_minutes_to_graduate']:>10.0f} min")
    print(f"   90th percentile   {funnel['p90_minutes_to_graduate']:>10.0f} min\n")

    print("2. Graduation rate by bundled buys at launch")
    for _, row in bundles.iterrows():
        print(f"   {row['bundled_buys']:>3} bundled   {int(row['launches']):>7,} launches   "
              f"{int(row['graduated']):>4} graduated   {row['graduation_rate_pct']:>6.2f} %")

    print("\n3. Price after migration (vs. the PumpSwap pool's opening price)")
    if returns.empty:
        print("   No PumpSwap trades found for the graduates in this window.")
        return
    print(f"   Graduates with PumpSwap trades: {len(returns):,}")
    for label in CHECKPOINTS:
        col = returns[f"return_{label}_pct"].dropna()
        print(f"   +{label:<4} median {col.median():>7.1f} %   "
              f"above opening price: {(col > 0).mean() * 100:>5.1f} % of tokens")


def save_charts(funnel, graduates, bundles, returns):
    OUT.mkdir(exist_ok=True)
    blue, grey = "#2f6fed", "#9aa5b1"

    fig, ax = plt.subplots(figsize=(6, 4))
    ax.bar(["Launched", "Graduated"], [funnel["launches"], funnel["graduated"]], color=[grey, blue])
    ax.set_yscale("log")
    ax.set_ylabel("Tokens (log scale)")
    ax.set_title(f"Pump.fun graduation funnel: {funnel['graduation_rate_pct']:.2f} % graduate")
    fig.tight_layout()
    fig.savefig(OUT / "funnel.png", dpi=150)

    fig, ax = plt.subplots(figsize=(7, 4))
    minutes = graduates["minutes_to_graduate"].clip(lower=1)
    bins = [1, 2, 5, 10, 30, 60, 180, 360, 720, 1440, 2880]
    ax.hist(minutes, bins=bins, color=blue)
    ax.set_xscale("log")
    ax.set_xticks(bins, ["1m", "2m", "5m", "10m", "30m", "1h", "3h", "6h", "12h", "1d", "2d"])
    ax.set_xlabel("Time from creation to migration")
    ax.set_ylabel("Graduated tokens")
    ax.set_title("Time to graduation")
    fig.tight_layout()
    fig.savefig(OUT / "time_to_graduation.png", dpi=150)

    fig, ax = plt.subplots(figsize=(6, 4))
    ax.bar(bundles["bundled_buys"].astype(str), bundles["graduation_rate_pct"], color=blue)
    ax.set_xlabel("Buys bundled into the launch transaction")
    ax.set_ylabel("Graduation rate (%)")
    ax.set_title("Graduation rate by bundled buys")
    fig.tight_layout()
    fig.savefig(OUT / "graduation_by_bundle.png", dpi=150)

    if not returns.empty:
        fig, ax = plt.subplots(figsize=(7, 4))
        series = [returns[f"return_{label}_pct"].dropna() for label in CHECKPOINTS]
        ax.boxplot(series, showfliers=False)
        ax.set_xticks([1, 2, 3], ["+5 min", "+1 h", "+24 h"])
        ax.axhline(0, color="#d64545", linewidth=1)
        ax.set_ylabel("Return vs. pool opening price (%)")
        ax.set_title("Price after migration")
        fig.tight_layout()
        fig.savefig(OUT / "post_migration_returns.png", dpi=150)

    graduates.to_csv(OUT / "graduates.csv", index=False)
    returns.to_csv(OUT / "post_migration.csv", index=False)


def main():
    client = get_client()
    start, end = study_window(client)
    params = {
        "start": f"{start:%Y-%m-%d %H:%M:%S}",
        "end": f"{end:%Y-%m-%d %H:%M:%S}",
        "horizon": HORIZON_DAYS,
    }

    funnel = client.query_df(queries.FUNNEL, params).iloc[0]
    graduates = client.query_df(queries.GRADUATES, params)
    bundles = client.query_df(queries.BY_BUNDLE, params)
    returns = post_migration_returns(client, params)

    print_report(start, end, funnel, bundles, returns)
    save_charts(funnel, graduates, bundles, returns)
    print(f"\nCharts and CSVs written to {OUT.resolve()}")


if __name__ == "__main__":
    main()
```
{% endcode %}

`study_window` is the most important function here. If the window ended today, tokens launched in the last hour would count as "never graduated" simply because they have not had time yet, and the graduation rate would come out too low. Ending the window `HORIZON_DAYS` before the newest data gives every launch the same chance.
{% endstep %}

{% step %}
### Run the research

```bash
python research.py
```

The script runs five queries, and completes in a few seconds:

```bash
Window: launches from 2026-11-22 00:00 to 2026-11-23 00:00 UTC
Horizon: 2 days to graduate, 24 h of trading after migration

1. Graduation funnel
   Launches              36,118
   Graduated                542
   Graduation rate         1.50 %
   Median time                4 min
   90th percentile          162 min

2. Graduation rate by bundled buys at launch
     0 bundled     3,653 launches      2 graduated     0.05 %
     1 bundled    26,836 launches    327 graduated     1.22 %
   2-4 bundled     4,330 launches     75 graduated     1.73 %
    5+ bundled     1,299 launches    138 graduated    10.62 %

3. Price after migration (vs. the PumpSwap pool's opening price)
   Graduates with PumpSwap trades: 542
   +5m   median   -50.2 %   above opening price:  33.9 % of tokens
   +1h   median   -95.0 %   above opening price:  13.8 % of tokens
   +24h  median   -95.4 %   above opening price:   4.1 % of tokens

Charts and CSVs written to /path/to/pumpfun-graduation-research/out
```

{% hint style="info" %}
**One day is a snapshot.** Results move from day to day, so widen the window before drawing conclusions. On the Monthly plan, `WINDOW_DAYS=30` runs in under a minute. Over the 30 days to 2026-11-23, 1,266,813 launches produced 25,496 graduates:

| Finding                          | 1 day  | 30 days |
| -------------------------------- | ------ | ------- |
| Graduation rate                  | 1.50%  | 2.01%   |
| Median time to graduate          | 4 min  | 3 min   |
| Graduation rate, 5+ bundled buys | 10.62% | 14.98%  |
| Median return at +5 min          | −50.2% | −15.6%  |
| Median return at +1 h            | −95.0% | −94.9%  |
| Above opening price at +24 h     | 4.1%   | 6.0%    |

The direction of every finding holds across both windows, but the +5-minute return is the least stable. Also check that every graduate has PumpSwap trades (`Graduates with PumpSwap trades` equals `Graduated`): tokens without trades drop out of the returns and would bias them upward.
{% endhint %}
{% endstep %}
{% endstepper %}

## Troubleshooting

<table data-search="false"><thead><tr><th>Symptom</th><th>Likely cause</th><th>Fix</th></tr></thead><tbody><tr><td><code>Set GETBLOCK_USER_ID and GETBLOCK_API_KEY in .env</code></td><td><code>.env</code> is missing or still empty</td><td>Copy <code>.env.example</code> to <code>.env</code> and fill both values in.</td></tr><tr><td><code>Market SQL error 401</code></td><td>The user ID and API key belong to different accounts, or the key was deleted</td><td>Copy both from the same account's <a href="https://account.getblock.io/products/sql#api-docs">API Docs tab</a>.</td></tr><tr><td><code>Market SQL error 403</code> / access denied</td><td>No active trial or subscription for the Solana dataset</td><td>Activate Solana under <strong>Products → Market SQL</strong>. Each dataset is subscribed to separately.</td></tr><tr><td><code>Database is required</code></td><td>The request was sent without the <code>database</code> parameter</td><td>Keep <code>database=solana</code> on every request. Some ClickHouse clients send setup queries without it, which is why this guide uses a plain HTTPS client.</td></tr><tr><td><code>TOO_MANY_SIMULTANEOUS_QUERIES</code></td><td>Trial plans run one query at a time, and an earlier query is still running, even if your client gave up on it</td><td>Wait for it to finish, then retry. Always bound queries by <code>block_time</code> so none run for long.</td></tr><tr><td><code>TIMEOUT_EXCEEDED</code> / query stopped</td><td>The window is too wide for your plan's execution limit</td><td>Keep <code>WINDOW_DAYS=1</code> on the trials (10 s per query). On the Monthly plan (60 s per query), <code>WINDOW_DAYS=30</code> fits.</td></tr><tr><td><code>Missing columns</code> / <code>Unknown identifier</code></td><td>A column name differs from the one used in <code>queries.py</code></td><td>Run <code>python inspect_tables.py</code> and update the name in <code>queries.py</code>.</td></tr><tr><td>A join returns no matches</td><td>Comparing <code>FixedString</code> mints with <code>String</code> mints</td><td>Convert with <code>toString(mint)</code> before joining, as <code>launches</code> does.</td></tr><tr><td>Graduation rate looks too low</td><td>The window is too close to the newest data</td><td>Increase <code>HORIZON_DAYS</code> so every launch has longer to graduate.</td></tr><tr><td>Returns of thousands of percent</td><td>A graduate with only a handful of trades</td><td>Filter <code>post_migration.csv</code> on <code>swaps_24h</code>, for example to tokens with at least 50 trades.</td></tr></tbody></table>

## Conclusion

You built a Pump.fun graduation study that runs entirely on GetBlock Market SQL: one shared launch cohort, server-side aggregation for the funnel and the bundle breakdown, and `argMaxIf` checkpoints against the pool's opening price for post-migration returns.&#x20;

The key techniques were placing the window behind the newest data so every launch has a fair chance to graduate, bounding every table by time, normalizing mint types before joining, and ordering trades by slot and transaction index rather than by timestamp.

## Resources

* [Market SQL overview](https://app.gitbook.com/s/FOeg95CadVyFvyLi70Bh/market-sql)
* [Market SQL Getting Started](../market-sql/getting-started.md)
* [Querying Market SQL](../market-sql/querying-market-sql.md)
* [Solana dataset table reference](../market-sql/api-reference/solana.md#tables)
* [Get your Market SQL credentials](https://account.getblock.io/products/sql#api-docs)
* [ClickHouse queries with parameters over HTTP](https://clickhouse.com/docs/interfaces/http#cli-queries-with-parameters)
* [GitHub project repo](https://github.com/GetBlock-io/guides/tree/main/pumpfun-graduation-research)
