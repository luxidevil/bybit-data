<!-- gold-data-status:start -->
# Gold bid/ask datasets

Two separate feeds. They must not be merged or treated as the same instrument.

| Dataset | Range | Native detail | Status |
|---|---|---|---|
| Bybit XAUUSDT linear perpetual | Archive dates 2026-03-09–2026-10-23 | Every depth-200 archive event; nominal 100 ms | 229/229 archives converted; 110,694,329 rows. Full release upload pending. |
| Dukascopy XAUUSD spot gold | Requested 2016-10-24 00:00 UTC to 2026-10-24 00:00 UTC (exclusive) | Every original bid/ask tick; no sampling | Ten-year download and coverage audit in progress. |

## Files currently in this repository

- [Bybit format and source notes](datasets/bybit-xauusdt/README.md)
- [Bybit complete coverage manifest](datasets/bybit-xauusdt/manifest.json)
- [Bybit daily file index](datasets/bybit-xauusdt/daily-file-index.csv)
- [Bybit preview — 1,000 quotes only](datasets/bybit-xauusdt/preview_first_1000_quotes.csv)
- [Dukascopy format and source notes](datasets/dukascopy-xauusd/README.md)
- [Dukascopy preview — 1,000 ticks only](datasets/dukascopy-xauusd/preview_first_1000_ticks.csv)

**The previews are not the complete datasets.** The complete Bybit export has now been split into 20 independently usable ZIP parts, all below 90 MiB, with all 229 daily files and 110,694,329 native events verified. The parts have been delivered in the Replit Library. GitHub release-attachment upload is still pending authorization; no bulk ZIP files have yet been uploaded to GitHub.

- [Bybit ZIP part index, sizes, dates and checksums](datasets/bybit-xauusdt/parts/part-index.json)
- [How to use the 20 parts](datasets/bybit-xauusdt/parts/README.md)
- [Part checksums](datasets/bybit-xauusdt/parts/SHA256SUMS)

The verified Bybit ZIP is 1,741,052,298 bytes. SHA-256:
`2b27eef545953682b11ab4a4a4d1a7b058d8047042eb3b688843b6ed2d3e6578`

An archive may include boundary messages just beyond its date: the final Bybit event is 2026-10-24T00:00:01.992Z. Empty or one-sided initial books are flagged; quotes are not fabricated. Missing archive dates and missing feed events are different checks.

The older file under data/raw/bybit/xauusdt_1m.csv is a legacy candle sample, not bid/ask data.
<!-- gold-data-status:end -->
