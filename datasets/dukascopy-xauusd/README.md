# Dukascopy XAUUSD — native bid/ask ticks

This is **Dukascopy spot gold quoted in USD**, not the Bybit XAUUSDT perpetual.
The two datasets are intentionally kept in separate directories and files.

The requested ten-year window is **2016-10-24 00:00:00 UTC inclusive through
2026-10-24 00:00:00 UTC exclusive**. This includes the final complete UTC day
before the request, October 23, 2026.

## Source and resolution

Public hourly source pattern:

```
https://datafeed.dukascopy.com/datafeed/XAUUSD/YYYY/MM/DD/HHh_ticks.bi5
```

Source months are zero-based (`00` = January). Each LZMA-compressed file contains
20-byte big-endian records: `uint32 offset_ms`, `uint32 ask`, `uint32 bid`,
`float32 ask_volume`, `float32 bid_volume`.

**For this hourly HTTP format, offsets are measured from the start of the
hour.** Do not apply the newer daily S3 format's start-of-day convention here.

Every source record is retained, including repeated prices, repeated timestamps,
and any source timestamp regressions. No candles, interval sampling, deduplication,
or invented quotes are used. Millisecond timestamps do not imply a quote every
millisecond. Sampled source files in 2016, 2021, and 2026 contained variable tick
intervals, with a smallest positive interval of 50 ms in those samples.

Prices use the XAUUSD scale **1000**, so integer `1265239` means `1265.239 USD`.
The scale is instrument-specific, not the default forex factor of 100,000.

References:

- [Dukascopy format documentation](https://www.dukascopy.com/wiki/en/development/data-export/)
- [XAUUSD history availability](https://www.dukascopy-node.app/instrument/xauusd)
- [Independent XAUUSD price-scale reference](https://pypi.org/project/tick-vault/)

## CSV columns

| Column | Meaning |
|---|---|
| `timestamp_ms` | Original tick timestamp as UTC Unix milliseconds |
| `bid_price` | Bid in USD, exact three-decimal source scaling |
| `ask_price` | Ask in USD, exact three-decimal source scaling |
| `bid_volume_native` | Original source bid volume; no unit conversion |
| `ask_volume_native` | Original source ask volume; no unit conversion |
| `spread` | Exact ask minus bid in USD |
| `tick_index_in_hour` | Zero-based source-record position, retaining duplicates |
| `quote_status` | `ok`, `locked`, `crossed`, or `zero_price` |

Volumes retain the decoded float32 values, represented without precision loss.
Do not treat them as Bybit order-book quantities or assume cross-feed units match.

## Coverage and integrity

The downloader checks all 24 hours of every requested date, including weekends.
Each daily sidecar audits all hourly requests, source URLs, response states,
byte counts and SHA-256 checksums. A zero-byte or compressed-empty source file
produces no quotes. HTTP 404/410 is recorded as `not_found`, not silently treated
as proof of a market closure. Failed requests are reported separately.

`download_processing_complete` means the requested traversal finished without
unresolved processing errors. It is **not** a guarantee that the provider's own
archive never lost a quote. Consult the hour-level audit for absent data.
No missing period is filled with fabricated records.

Downloads use a shared request budget, bounded concurrency and retries, and
global cooldown/reduced request rates after HTTP 429. A restart verifies and
reuses completed days rather than starting over.

## Reproduce

```bash
uv sync
uv run python scripts/download_dukascopy_ticks.py \
  --start 2016-10-24 --end 2026-10-24
```

Output lives in `generated-artifacts/dukascopy-xauusd-ticks/`, separate from
`generated-artifacts/bybit-bid-ask/`. Large generated datasets are excluded from
Git history; distribution archives belong in GitHub release assets, with
checksums and source attribution. Source data remains subject to the provider's
terms; no new license over the source quotes is asserted.
