# Dukascopy XAUUSD — native bid/ask tick history

**The full downloaded dataset is uploaded here: 528,248,304 original ticks across 3,652 UTC days.**
Range: **2016-10-24 00:00 UTC inclusive to 2026-10-24 00:00 UTC exclusive**.
This is **spot gold quoted in USD**, separate from Bybit XAUUSDT linear perpetual data.

## Which files contain the actual data?

- **[Download all 358 data ZIP chunks, or choose dates](DOWNLOADS.md)**
- [Browse the chunks folder](chunks/)
- [Find a specific day and its enclosing ZIP](daily-file-index.csv)
- [Machine-readable chunk index](chunk-index.json)
- [ZIP checksums](chunks/SHA256SUMS)

All 358 data ZIPs together contain the full export, about **6.16 GB**.
Each ZIP is independent and **under 22 MiB**, small enough for this repository's API upload path.
This repository also has approximately 1.74 GB of separate Bybit data; a full repository clone is roughly 7.9 GB.
Use individual ZIPs to download only the date ranges you need.
The files are repository contents, not release attachments.

Extract the data ZIPs into the same directory. Their `daily/` folders contain:

- `YYYY-MM-DD_XAUUSD_dukascopy_ticks.csv.gz` — actual bid/ask ticks.
- `YYYY-MM-DD_metadata.json` — hourly source audit and file hashes.

Each ZIP also includes its own `metadata/*.manifest.json`.
CSV.gz bytes are unchanged from the verified source export. Read them with gzip-aware tools or decompress them.
From the downloaded `chunks/` directory, verify ZIPs with `sha256sum -c SHA256SUMS`.
The checksum list also includes the separate coverage-metadata ZIP.

## Native detail and columns

```csv
timestamp_ms,bid_price,ask_price,bid_volume_native,ask_volume_native,spread,tick_index_in_hour,quote_status
```

Every original provider tick is retained, with millisecond UTC timestamps. There is no resampling,
candle substitution, interpolation or deduplication. Tick intervals vary; millisecond timestamp precision
does not mean a fixed 1 ms feed. Prices preserve the source integers divided by 1,000.
Volume fields retain the provider's native float32 values and units; they are not Bybit order-book sizes.

```python
import csv
import gzip

with gzip.open("daily/2016-10-24_XAUUSD_dukascopy_ticks.csv.gz", "rt", newline="") as file:
    reader = csv.DictReader(file)
    first_tick = next(reader)
    print(first_tick["timestamp_ms"], first_tick["bid_price"], first_tick["ask_price"])
```

## Coverage and limitations

- **3,652/3,652 calendar days and 87,648/87,648 source hours processed; zero unresolved download failures.**
- **59,164 hours contained ticks; 28,484 returned empty responses.**
- First tick: `2016-10-24T00:00:00.244Z`.
- Last tick: `2026-10-23T20:59:58.337Z`.
- Empty source hours are recorded explicitly, not filled with invented quotes.
- Complete processing does **not** prove the provider never lost observations. Upstream completeness is not guaranteed.
- [Full coverage manifest](manifest.json) and [all source-hour audit records](chunks/source-hour-audit.csv.gz).
- The coverage-metadata ZIP and `preview_first_1000_ticks.csv` are **not** substitutes for the data ZIPs.
- Do not splice this feed into Bybit data or label it Bybit XAUUSDT.

Source: [Dukascopy data export documentation](https://www.dukascopy.com/wiki/en/development/data-export/).
The legacy hourly `.bi5` source uses zero-based URL months and millisecond offsets from the start of each hour.
