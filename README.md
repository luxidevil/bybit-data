<!-- gold-data-status:start -->
# Gold bid/ask datasets

Two separate feeds. They must not be merged or treated as the same instrument.

| Dataset | Range | Native detail | Status |
|---|---|---|---|
| Bybit XAUUSDT linear perpetual | Archive dates 2026-03-09–2026-10-23 | Every depth-200 archive event; nominal 100 ms | COMPLETE: all 229 daily data files uploaded; 110,694,329 original records. |
| Dukascopy XAUUSD spot gold | Requested 2016-10-24 00:00 UTC to 2026-10-24 00:00 UTC (exclusive) | Every original bid/ask tick; no sampling | Ten-year download and coverage audit still in progress. Only a preview is uploaded. |

## Download the COMPLETE Bybit bid/ask data

**The complete Bybit dataset is now stored in this repository—not just previews or download instructions.**

- [Open all 229 daily CSV.gz data files and their metadata](datasets/bybit-xauusdt/daily/)
- [Daily file index with row counts and checksums](datasets/bybit-xauusdt/daily-file-index.csv)
- [Complete coverage and quality manifest](datasets/bybit-xauusdt/manifest.json)
- [SHA-256 checksums for the complete export](datasets/bybit-xauusdt/SHA256SUMS)
- [CSV columns, source documentation and Python reading example](datasets/bybit-xauusdt/README.md)

Each daily `.csv.gz` is a losslessly compressed CSV with the original timestamps, best bid and ask, sizes, spread and source update identifiers. No resampling was applied. The smallest historical detail available in these archives is the native depth-200 stream (nominal **100 ms**, with variable actual intervals), not the live-only 10 ms feed.

### Download all days

The compressed daily CSV files total **1,740,263,138 bytes (about 1.74 GB)**. A full download or clone is correspondingly large. To download individual days, open the daily folder above and use the file's raw/download link.

```bash
git clone --depth 1 https://github.com/luxidevil/bybit-data.git
cd bybit-data/datasets/bybit-xauusdt
sha256sum -c SHA256SUMS
```

The `.gz` files can be opened directly by Python gzip/pandas or decompressed with `gunzip`. The uploaded daily files are byte-for-byte identical to the verified local export.

### Coverage notes

- First two-sided quote: **2026-03-09T02:41:07.799Z**. There is no Bybit XAUUSDT history for 1–8 March in this source.
- Last source event: **2026-10-24T00:00:01.992Z**. Day-labelled archives can contain boundary messages beyond midnight and can overlap.
- All 229 listed daily archives are included. This does not prove that the upstream feed never had an outage.
- Empty and one-sided initial books are retained and flagged; missing quotes are not fabricated.
- The [20 ZIP-part indexes](datasets/bybit-xauusdt/parts/) describe the separately supplied Replit Library ZIP downloads. On GitHub, the full data is supplied as daily CSV.gz files instead of release attachments.

## Previews and the separate ten-year feed

- [Bybit preview—1,000 quotes only](datasets/bybit-xauusdt/preview_first_1000_quotes.csv)
- [Dukascopy format and source notes](datasets/dukascopy-xauusd/README.md)
- [Dukascopy preview—1,000 ticks only](datasets/dukascopy-xauusd/preview_first_1000_ticks.csv)

The older file under `data/raw/bybit/xauusdt_1m.csv` is a legacy candle sample, not bid/ask data. Neither preview should be confused with a complete dataset.
<!-- gold-data-status:end -->
