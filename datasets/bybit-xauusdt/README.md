# XAUUSDT historical best bid and ask

## Source and resolution

This dataset is for **Bybit's XAUUSDT linear USDT perpetual**, not spot forex
XAU/USD and not another venue's gold product.

Source: <https://quote-saver.bycsi.com/orderbook/linear/XAUUSDT/>

The directory exposes `XAUUSDT_ob200.data.zip` daily archives. Each contains
Bybit's native `orderbook.200.XAUUSDT` snapshot/delta messages.

The exporter emits **one row for every source message**. It does not sample to
one minute, one second, or any other clock interval. It also retains messages
that change deeper price levels without changing the best bid/ask.

Bybit currently documents:

- Live level 1: 10 ms nominal push frequency.
- Live level 200: 100 ms nominal push frequency.
- REST order book: a current snapshot, with no historical start/end parameters.

The 10 ms live feed cannot be reconstructed from the available 200-level
historical archives. **These files must not be described as 10 ms history.**
Actual archive timestamp differences vary; the manifest reports their observed
distribution. Millisecond timestamp precision is not a 1 ms sampling rate.

Official documentation:

1. <https://bybit-exchange.github.io/docs/v5/websocket/public/orderbook>
2. <https://bybit-exchange.github.io/docs/v5/market/orderbook>
3. <https://www.bybit.com/derivatives/en/history-data>

## Reconstructing quotes

For each archive, processing begins with a snapshot. Both book sides are reset
on every snapshot and when update ID `u` is 1, as specified by Bybit.

Incremental updates are applied to the price-level map:

- Size zero: delete that price level.
- New price: insert the level.
- Existing price: replace its size.

The output bid is the **highest remaining bid**, and the output ask is the
**lowest remaining ask**. A delta's first entry is NOT necessarily the best
price. Decimal arithmetic is used; there is no floating-point rounding of
prices or quantities. Spread is `ask_price - bid_price`.

## Columns

| Column | Meaning |
| --- | --- |
| `timestamp_ms` | Original Bybit message-generation time, Unix milliseconds (UTC). |
| `engine_timestamp_ms` | Original matching-engine `cts`, Unix milliseconds; blank if absent. |
| `bid_price` | Highest displayed buy price after this update, in USDT. |
| `bid_size` | Size at the best bid, in the source instrument's native quantity units. |
| `ask_price` | Lowest displayed sell price after this update, in USDT. |
| `ask_size` | Size at the best ask, in the source instrument's native quantity units. |
| `spread` | Exact ask minus bid; blank when either side is missing. |
| `update_id` | Native `u` update ID. |
| `sequence` | Native cross-sequence `seq`; not assumed consecutive. |
| `message_type` | Native `snapshot` or `delta`. |
| `book_status` | `ok`, `empty`, `one_sided`, `locked`, or `crossed`. |

An empty book or missing side is represented with blank quote fields, not zero
prices or fabricated quotes. Locked/crossed states are retained and flagged,
not silently repaired or excluded.

## Coverage and quality

`manifest.json` records exact observed first/last timestamps, archive coverage,
event counts, quote-state counts, duplicate/backward timestamp diagnostics,
source URLs, and SHA-256 hashes of both input archives and output files.

The maximum range is the full set of daily archives actually listed by Bybit
when discovery runs. There is no artificial lookback cap or first-1,000-row
limit. Today's incomplete archive may not yet be published.

Important limitations:

- Calendar coverage does **not** prove that the source feed has no outages.
- Day-labelled source archives can overlap past UTC midnight. Each output
  preserves that source archive's order and boundaries. Inspect
  `archive_boundaries` in the manifest; do not blindly concatenate day files
  and assume a duplicate-free chronological stream.
- Do not drop all records with the same timestamp: separate native messages
  can share a millisecond. Use update IDs, cross-sequence, message type and
  quote content when reconciling overlaps.
- There can be an initial period with no two-sided market. The manifest
  distinguishes the first event from the first two-sided quote.
- Bybit excludes Retail Price Improvement (RPI) orders from this feed.
- Best quotes and top-level sizes are exported, not all 200 price levels.
  The manifest links the original full-depth source archives.
- Data is supplied for research. No profit or execution-quality claims are made.

## Files

- `daily/YYYY-MM-DD_XAUUSDT_bid_ask.csv.gz`: full-resolution quotes for each
  source archive, compressed without loss of precision.
- `daily/YYYY-MM-DD_metadata.json`: per-file quality statistics and hashes.
- `manifest.json`: overall coverage, schema, quality diagnostics and provenance.
- `source-index.json`: exact archive names discovered for this run.
- `daily-file-index.csv`: a compact human-readable inventory.
- `preview_first_1000_quotes.csv`: a small preview of two-sided quotes only;
  the full daily files also retain empty and one-sided events.
- `SHA256SUMS`: checksums for exported content.

CSV files are compressed because the complete event-level dataset is large.
They can be read directly without first extracting the CSV to disk:

```python
import csv
import gzip
from datetime import datetime, timezone

with gzip.open("daily/2026-03-09_XAUUSDT_bid_ask.csv.gz", "rt", newline="") as f:
    for row in csv.DictReader(f):
        if row["book_status"] != "ok":
            continue
        utc_time = datetime.fromtimestamp(
            int(row["timestamp_ms"]) / 1000, timezone.utc
        )
        print(utc_time, row["bid_price"], row["ask_price"])
        break
```

## Reproduce or resume

From the imported Python project's root:

```bash
uv sync
uv run python scripts/download_bid_ask.py --workers 4
uv run python scripts/package_bid_ask.py
```

Default output is `generated-artifacts/bybit-bid-ask/`, excluded from Git and
deployment images. Downloaded ZIPs are removed after successful conversion.
Use `--keep-archives` to retain them. Re-running skips verified completed
exports; it checks their hashes rather than merely trusting file existence.

Optional `--start YYYY-MM-DD` and `--end YYYY-MM-DD` restrict the range.
Omit both for all available history.

Failures are explicit, partial files are not treated as completed exports,
and the overall job exits unsuccessfully if any archive fails.
